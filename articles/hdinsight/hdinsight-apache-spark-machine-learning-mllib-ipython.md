<properties 
	pageTitle="Apache Spark を使用した HDInsight での Machine Learning アプリケーションの作成 | Microsoft Azure" 
	description="Apache Spark で Notebook を使用して Machine Learning アプリケーションを作成する手順を説明します" 
	services="hdinsight" 
	documentationCenter="" 
	authors="nitinme" 
	manager="jhubbard" 
	editor="cgronlun"
	tags="azure-portal"/>

<tags 
	ms.service="hdinsight" 
	ms.workload="big-data" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="07/25/2016" 
	ms.author="nitinme"/>


# Machine Learning: HDInsight Linux での MLlib と Apache Spark クラスターを使用した食品検査データの予測分析

> [AZURE.TIP] このチュートリアルは、HDInsight で作成する Spark (Linux) クラスター上の Jupyter Notebook としても利用できます。Notebook エクスペリエンスにより、Notebook 自体から Python のスニペットを実行することができます。Notebook からチュートリアルを実行するには、Spark クラスターを作成し、Jupyter Notebook (`https://CLUSTERNAME.azurehdinsight.net/jupyter`) を起動し、Notebook の **Python** フォルダーにある**Spark Machine Learning - Predictive analysis on food inspection data using MLLib.ipynb (Machine learning: MLLib.ipynb を使用した食品検査データの予測分析)** を実行します。


この記事では、**MLLib**、Spark の組み込みの Machine Learning ライブラリを使用して、開いているデータセットに対して簡単な予測分析を実行する方法を示します。MLLib は、Machine Learning タスクに役立つ多数のユーティリティを提供する、コア Spark ライブラリです。これには、以下のことに適したユーティリティが含まれます。

* 分類

* 回帰

* クラスタリング

* トピックのモデリング

* 特異値分解 (SVD) と主成分分析 (PCA)

* 仮説テストとサンプル統計の計算

この記事では、ロジスティック回帰を使用した*分類*への簡単なアプローチを提供します。

## 分類およびロジスティック回帰とは

一般的な Machine Learning タスクである*分類*は、入力データをカテゴリに分類するプロセスです。ユーザーが指定した入力データに「ラベル」を割り当てる方法を決定するのは、分類アルゴリズムの仕事です。たとえば、株式情報を入力として受け取り、株式を、売却する必要のある株式と保持する必要のある株式の 2 つのカテゴリに分類する Machine Learning アルゴリズムを考えてみます。

ロジスティック回帰は、分類に使用するアルゴリズムです。Spark のロジスティック回帰 API は、*二項分類* (入力データを 2 つのグループのいずれかに分類する) に適しています。ロジスティック回帰の詳細については、[Wikipedia](https://en.wikipedia.org/wiki/Logistic_regression) を参照してください。

要約すると、ロジスティック回帰のプロセスにより、入力ベクトルがどちらか 1 つのグループに属している確率を予測するために使用できる*ロジスティック関数*が生成されます。

## この記事の目的

Spark を使用して、[シカゴ市のデータ ポータル](https://data.cityofchicago.org/)から取得した食品検査データ (**Food\_Inspections1.csv**) に対していくつかの予測分析を実行します。このデータセットには、シカゴで実施された食品検査に関する情報が含まれており、検査された各食品施設に関する情報、見つかった違反 (存在する場合)、検査結果についての情報が含まれています。CSV データ ファイルは、クラスターに関連付けられたストレージ アカウントの **/HdiSamples/HdiSamples/FoodInspectionData/Food\_Inspections1.csv** に既に用意されています。

次の手順で、食品検査に合格または不合格になる理由を示すモデルを作成します。

## Spark MLlib を使用して Machine Learning アプリケーション作成を開始する

1. [Azure ポータル](https://portal.azure.com/)のスタート画面で Spark クラスターのタイルをクリックします (スタート画面にピン留めしている場合)。**[すべて参照]** > **[HDInsight クラスター]** でクラスターに移動することもできます。

2. Spark クラスター ブレードで、**[クイック リンク]** をクリックし、**[クラスター ダッシュボード]** ブレードで **[Jupyter Notebook]** をクリックします。入力を求められたら、クラスターの管理者資格情報を入力します。

	> [AZURE.NOTE] ブラウザーで次の URL を開き、クラスターの Jupyter Notebook にアクセスすることもできます。__CLUSTERNAME__ をクラスターの名前に置き換えます。
	>
	> `https://CLUSTERNAME.azurehdinsight.net/jupyter`

2. 新しい Notebook を作成します。**[新規]** をクリックし、**[PySpark]** をクリックします。

	![新しい Jupyter Notebook を作成します](./media/hdinsight-apache-spark-machine-learning-mllib-ipython/hdispark.note.jupyter.createnotebook.png "新しい Jupyter Notebook を作成します")

3. Untitled.pynb という名前の新しい Notebook が作成されて開かれます。上部の Notebook 名をクリックし、わかりやすい名前を入力します。

	![Notebook の名前を指定します](./media/hdinsight-apache-spark-machine-learning-mllib-ipython/hdispark.note.jupyter.notebook.name.png "Notebook の名前を指定します")

3. PySpark カーネルを使用して Notebook を作成したため、コンテキストを明示的に作成する必要はありません。最初のコード セルを実行すると、Spark および Hive コンテキストが自動的に作成されます。Machine Learning アプリケーションの作成を始めるには、このシナリオに必要な種類をインポートします。これを行うには、セルにカーソルを置き、**SHIFT + ENTER** キーを押します。


		from pyspark.ml import Pipeline
		from pyspark.ml.classification import LogisticRegression
		from pyspark.ml.feature import HashingTF, Tokenizer
		from pyspark.sql import Row
		from pyspark.sql.functions import UserDefinedFunction
		from pyspark.sql.types import *

## 入力データフレームを作成する

`sqlContext` を使用すると、構造化データに対して変換を実行できます。最初のタスクは、サンプル データ ((**Food\_Inspections1.csv**))を Spark SQL *データフレーム*に読み込むことです。

1. 生のデータが CSV 形式であるため、Spark コンテキストを使用して、ファイルのすべての行を非構造化テキストとしてメモリにプルする必要があります。次に、Python の CSV ライブラリを使用して、各行を個別に解析します。


		def csvParse(s):
		    import csv
		    from StringIO import StringIO
		    sio = StringIO(s)
		    value = csv.reader(sio).next()
		    sio.close()
		    return value
		
		inspections = sc.textFile('wasbs:///HdiSamples/HdiSamples/FoodInspectionData/Food_Inspections1.csv')\
		                .map(csvParse)


2. これで、CSV ファイルを RDD として使用できます。RDD から 1 つの行を取得して、データのスキーマを理解します。


		inspections.take(1)


	出力は次のように表示されます。

	    # -----------------
		# THIS IS AN OUTPUT
		# -----------------

		[['413707',
	      'LUNA PARK INC',
	      'LUNA PARK  DAY CARE',
	      '2049789',
	      "Children's Services Facility",
	      'Risk 1 (High)',
	      '3250 W FOSTER AVE ',
	      'CHICAGO',
	      'IL',
	      '60625',
	      '09/21/2010',
	      'License-Task Force',
	      'Fail',
	      '24. DISH WASHING FACILITIES: PROPERLY DESIGNED, CONSTRUCTED, MAINTAINED, INSTALLED, LOCATED AND OPERATED - Comments: All dishwashing machines must be of a type that complies with all requirements of the plumbing section of the Municipal Code of Chicago and Rules and Regulation of the Board of Health. OBSEVERD THE 3 COMPARTMENT SINK BACKING UP INTO THE 1ST AND 2ND COMPARTMENT WITH CLEAR WATER AND SLOWLY DRAINING OUT. INST NEED HAVE IT REPAIR. CITATION ISSUED, SERIOUS VIOLATION 7-38-030 H000062369-10 COURT DATE 10-28-10 TIME 1 P.M. ROOM 107 400 W. SURPERIOR. | 36. LIGHTING: REQUIRED MINIMUM FOOT-CANDLES OF LIGHT PROVIDED, FIXTURES SHIELDED - Comments: Shielding to protect against broken glass falling into food shall be provided for all artificial lighting sources in preparation, service, and display facilities. LIGHT SHIELD ARE MISSING UNDER HOOD OF  COOKING EQUIPMENT AND NEED TO REPLACE LIGHT UNDER UNIT. 4 LIGHTS ARE OUT IN THE REAR CHILDREN AREA,IN THE KINDERGARDEN CLASS ROOM. 2 LIGHT ARE OUT EAST REAR, LIGHT FRONT WEST ROOM. NEED TO REPLACE ALL LIGHT THAT ARE NOT WORKING. | 35. WALLS, CEILINGS, ATTACHED EQUIPMENT CONSTRUCTED PER CODE: GOOD REPAIR, SURFACES CLEAN AND DUST-LESS CLEANING METHODS - Comments: The walls and ceilings shall be in good repair and easily cleaned. MISSING CEILING TILES WITH STAINS IN WEST,EAST, IN FRONT AREA WEST, AND BY THE 15MOS AREA. NEED TO BE REPLACED. | 32. FOOD AND NON-FOOD CONTACT SURFACES PROPERLY DESIGNED, CONSTRUCTED AND MAINTAINED - Comments: All food and non-food contact equipment and utensils shall be smooth, easily cleanable, and durable, and shall be in good repair. SPLASH GUARDED ARE NEEDED BY THE EXPOSED HAND SINK IN THE KITCHEN AREA | 34. FLOORS: CONSTRUCTED PER CODE, CLEANED, GOOD REPAIR, COVING INSTALLED, DUST-LESS CLEANING METHODS USED - Comments: The floors shall be constructed per code, be smooth and easily cleaned, and be kept clean and in good repair. INST NEED TO ELEVATE ALL FOOD ITEMS 6INCH OFF THE FLOOR 6 INCH AWAY FORM WALL.  ',
	      '41.97583445690982',
	      '-87.7107455232781',
	      '(41.97583445690982, -87.7107455232781)']]


3. 上記の出力により、入力ファイルのスキーマのアイデアが提供されます。ファイルには、すべての施設名、施設の種類、アドレス、検査のデータ、場所などが含まれています。予測分析に役立ついくつかの列を選択し、結果をデータフレームとしてグループ化しましょう。次に、これを使って一時テーブルを作成します。


		schema = StructType([
        StructField("id", IntegerType(), False), 
        StructField("name", StringType(), False), 
        StructField("results", StringType(), False), 
        StructField("violations", StringType(), True)])

		df = sqlContext.createDataFrame(inspections.map(lambda l: (int(l[0]), l[1], l[12], l[13])) , schema)
		df.registerTempTable('CountResults')

4. これで、データフレーム、`df` を取得できました。ここで分析を実行できます。**CountResults** という一時テーブルも作成できました。データフレームで対象の 4 列 (**id**、**name**、**results**、および **violations**) が含まれています。
	
	データの小さなサンプルを取得します。

		df.show(5)

	出力は次のように表示されます。

	    # -----------------
		# THIS IS AN OUTPUT
		# -----------------

		+------+--------------------+-------+--------------------+
	    |    id|                name|results|          violations|
	    +------+--------------------+-------+--------------------+
	    |413707|       LUNA PARK INC|   Fail|24. DISH WASHING ...|
	    |391234|       CAFE SELMARIE|   Fail|2. FACILITIES TO ...|
	    |413751|          MANCHU WOK|   Pass|33. FOOD AND NON-...|
	    |413708|BENCHMARK HOSPITA...|   Pass|                    |
	    |413722|           JJ BURGER|   Pass|                    |
	    +------+--------------------+-------+--------------------+

## データを理解する

1. データセットの内容を理解しましょう。たとえば、**results** 列のさまざまな値は何でしょうか。


		df.select('results').distinct().show()

	
	出力は次のように表示されます。

	    # -----------------
		# THIS IS AN OUTPUT
		# -----------------
	
		+--------------------+
	    |             results|
	    +--------------------+
	    |                Fail|
	    |Business Not Located|
	    |                Pass|
	    |  Pass w/ Conditions|
	    |     Out of Business|
	    +--------------------+
    
2. グラフ化することで、これらの結果の分布の理解に役立ちます。一時テーブル **CountResults** には既にデータが入力されています。テーブルに対し次の SQL クエリを実行して、結果がどのように分布しているかを分析できます。

		%%sql -o countResultsdf
		SELECT results, COUNT(results) AS cnt FROM CountResults GROUP BY results

	`%%sql` マジックの後に `-o countResultsdf` と入力して、クエリの出力が Jupyter サーバー (通常はクラスターのヘッドノード) にローカルに保持されるようにします。出力は、指定された **countResultsdf** という名前で [Pandas](http://pandas.pydata.org/) データフレームとして保存されます。
	
	出力は次のように表示されます。
	
	![SQL クエリ出力](./media/hdinsight-apache-spark-machine-learning-mllib-ipython/query.output.png "SQL クエリ出力")

	`%%sql` マジックと、PySpark カーネルで使用できるその他のマジックの詳細については、「[HDInsight (Linux) の Spark クラスターと Jupyter Notebook で使用可能なカーネル](hdinsight-apache-spark-jupyter-notebook-kernels.md#why-should-i-use-the-new-kernels)」をご覧ください。

3. データの視覚効果の構築に使用するライブラリ、Matplotlib を使用して、プロットを作成することもできます。プロットはローカルに保存された **countResultsdf** データフレームから作成する必要があるため、コード スニペットは `%%local` マジックで始める必要があります。これにより、コードは Jupyter サーバーでローカルに実行されます。

		%%local
		%matplotlib inline
		import matplotlib.pyplot as plt
		
		
		labels = countResultsdf['results']
		sizes = countResultsdf['cnt']
		colors = ['turquoise', 'seagreen', 'mediumslateblue', 'palegreen', 'coral']
		plt.pie(sizes, labels=labels, autopct='%1.1f%%', colors=colors)
		plt.axis('equal')

	出力は次のように表示されます。

	![出力結果](./media/hdinsight-apache-spark-machine-learning-mllib-ipython/output_13_1.png)


4. 検査には、5 つの個別の結果があることを確認できます。
	
	* 事業体が存在しない
	* 不合格
	* 合格
	* 条件付きで合格
	* 廃業

	違反を考慮した、食品検査の結果を推測できるモデルを作成してみましょう。ロジスティック回帰は二項分類メソッドであるため、データを **Fail** と **Pass** の 2 つのカテゴリにグループ化することは意味があります。「Pass w/ Conditions」は Pass であるため、モデルをトレーニングするときは、この 2 つの結果が同等であると見なします。その他の結果のデータ (「Business Not Located」、「Out of Business」) は使用できないため、トレーニング セットから削除します。いずれにしても、これら 2 つのカテゴリが結果に占める割合は非常にわずかであるため、問題ありません。

5. 先に進み、既存のデータフレーム (`df`) を、各検査がラベルと違反のペアとして表される新しいデータフレームに変換します。ここでは、ラベル `0.0` は失敗、ラベル `1.0` は成功、ラベル `-1.0` はこれら 2 つ以外の何らかの結果であることを表します。新しいデータ フレームを計算するときに、これらのその他の結果は除外されます。


		def labelForResults(s):
		    if s == 'Fail':
		        return 0.0
		    elif s == 'Pass w/ Conditions' or s == 'Pass':
		        return 1.0
		    else:
		        return -1.0
		label = UserDefinedFunction(labelForResults, DoubleType())
		labeledData = df.select(label(df.results).alias('label'), df.violations).where('label >= 0')


	ラベル付けされたデータから 1 つの行を取得し、内容を確認してみましょう。


		labeledData.take(1)


	出力は次のように表示されます。
	
	    # -----------------
		# THIS IS AN OUTPUT
		# -----------------
	
		[Row(label=0.0, violations=u"41. PREMISES MAINTAINED FREE OF LITTER, UNNECESSARY ARTICLES, CLEANING  EQUIPMENT PROPERLY STORED - Comments: All parts of the food establishment and all parts of the property used in connection with the operation of the establishment shall be kept neat and clean and should not produce any offensive odors.  REMOVE MATTRESS FROM SMALL DUMPSTER. | 35. WALLS, CEILINGS, ATTACHED EQUIPMENT CONSTRUCTED PER CODE: GOOD REPAIR, SURFACES CLEAN AND DUST-LESS CLEANING METHODS - Comments: The walls and ceilings shall be in good repair and easily cleaned.  REPAIR MISALIGNED DOORS AND DOOR NEAR ELEVATOR.  DETAIL CLEAN BLACK MOLD LIKE SUBSTANCE FROM WALLS BY BOTH DISH MACHINES.  REPAIR OR REMOVE BASEBOARD UNDER DISH MACHINE (LEFT REAR KITCHEN). SEAL ALL GAPS.  REPLACE MILK CRATES USED IN WALK IN COOLERS AND STORAGE AREAS WITH PROPER SHELVING AT LEAST 6' OFF THE FLOOR.  | 38. VENTILATION: ROOMS AND EQUIPMENT VENTED AS REQUIRED: PLUMBING: INSTALLED AND MAINTAINED - Comments: The flow of air discharged from kitchen fans shall always be through a duct to a point above the roofline.  REPAIR BROKEN VENTILATION IN MEN'S AND WOMEN'S WASHROOMS NEXT TO DINING AREA. | 32. FOOD AND NON-FOOD CONTACT SURFACES PROPERLY DESIGNED, CONSTRUCTED AND MAINTAINED - Comments: All food and non-food contact equipment and utensils shall be smooth, easily cleanable, and durable, and shall be in good repair.  REPAIR DAMAGED PLUG ON LEFT SIDE OF 2 COMPARTMENT SINK.  REPAIR SELF CLOSER ON BOTTOM LEFT DOOR OF 4 DOOR PREP UNIT NEXT TO OFFICE.")]


## 入力データ フレームからロジスティック回帰モデルを作成する

最後のタスクは、ラベル付けされたデータをロジスティック回帰で分析できる形式に変換することです。ロジスティック回帰アルゴリズムへの入力は、ラベルと機能ベクトルのペアである必要があります。ここで「機能ベクトル」とは、何らかの方法での入力ポイントを表す数のベクトルです。そのため、半構造化され、多くのフリー テキストによるコメントを含む「violations」列を、コンピューターが簡単に理解できる実数の配列に変換する方法が必要です。

自然言語を処理するための 1 つの標準的な Machine Learning のアプローチは、個別の単語に「インデックス」を割り当て、ベクトルを Machine Learning アルゴリズムに渡し、各インデックスの値にその単語の相対度数がテキスト文字列で含まれるようにします。

MLLib では、この操作を実行する簡単な方法を提供します。最初に、各違反文字列を「トークン化」して各文字列の個々の単語を取得し、`HashingTF` を使用してトークンの各セットを機能ベクトルに渡します。これにより、ロジスティック回帰アルゴリズムに渡され、モデルを作成することができます。「パイプライン」を使用して、これらすべての手順を順番に実行します。


	tokenizer = Tokenizer(inputCol="violations", outputCol="words")
	hashingTF = HashingTF(inputCol=tokenizer.getOutputCol(), outputCol="features")
	lr = LogisticRegression(maxIter=10, regParam=0.01)
	pipeline = Pipeline(stages=[tokenizer, hashingTF, lr])
	
	model = pipeline.fit(labeledData)


## 別のテスト データセットでモデルを評価する

前に作成したモデルを使用し、どのくらい違反が観察されたかに基づいて、新しい検査結果を*予測*できます。データセット **Food\_Inspections1.csv** でこのモデルをトレーニングしました。2 つ目のデータセット **Food\_Inspections2.csv** を使用して、新しいデータでこのモデルの強度を*評価*します。この 2 つ目のデータ セット (**Food\_Inspections2.csv**) は、クラスターに関連付けられている既定のストレージ コンテナーに既に存在している必要があります。

1. 次のスニペットでは、モデルによって生成された予測を含む、新しいデータフレーム **predictionsDf** を作成します。このスニペットでは、データフレームに基づいた一時テーブル **Predictions** も作成します。


		testData = sc.textFile('wasbs:///HdiSamples/HdiSamples/FoodInspectionData/Food_Inspections2.csv')\
	             .map(csvParse) \
	             .map(lambda l: (int(l[0]), l[1], l[12], l[13]))
		testDf = sqlContext.createDataFrame(testData, schema).where("results = 'Fail' OR results = 'Pass' OR results = 'Pass w/ Conditions'")
		predictionsDf = model.transform(testDf)
		predictionsDf.registerTempTable('Predictions')
		predictionsDf.columns


	出力は次のように表示されます。
	
	    # -----------------
		# THIS IS AN OUTPUT
		# -----------------
		
		['id',
	     'name',
	     'results',
	     'violations',
	     'words',
	     'features',
	     'rawPrediction',
	     'probability',
	     'prediction']

2. 予測のいずれかを確認します。このスニペットを実行します。

		predictionsDf.take(1)

	テスト データ セットの最初のエントリの予測が表示されます。

3. `model.transform()` メソッドは、同じスキーマを持つ新しいデータに同じ変換を適用し、データの分類方法の予測に到達します。予測がどれだけ正確だったかを把握するための簡単な統計を実行できます。


		numSuccesses = predictionsDf.where("""(prediction = 0 AND results = 'Fail') OR 
		                                      (prediction = 1 AND (results = 'Pass' OR 
		                                                           results = 'Pass w/ Conditions'))""").count()
		numInspections = predictionsDf.count()
		
		print "There were", numInspections, "inspections and there were", numSuccesses, "successful predictions"
		print "This is a", str((float(numSuccesses) / float(numInspections)) * 100) + "%", "success rate"

	出力は次のようになります。
	
	    # -----------------
		# THIS IS AN OUTPUT
		# -----------------
	
		There were 9315 inspections and there were 8087 successful predictions
	    This is a 86.8169618894% success rate


	Spark を使用するロジスティック回帰の使用により、英語による違反の説明と、特定の会社が食品検査で合格か不合格かの関係を示す精密モデルが表示されます。

## 予測を視覚化する

このテスト結果の理解に役立つ最終的なグラフを作成します。

1. まず、先ほど作成した一時テーブル **Predictions** からさまざまな予測や結果を抽出します。次のクエリでは、出力を *true\_positive*、*false\_positive*、*true\_negative*、*false\_negative* に分けています。このクエリでは、`-q` を使用して視覚化を無効にし、`%%local` マジックで使用できるデータフレームとして出力を保存 (`-o` を使用) します。

		%%sql -q -o true_positive
		SELECT count(*) AS cnt FROM Predictions WHERE prediction = 0 AND results = 'Fail'

		%%sql -q -o false_positive
		SELECT count(*) AS cnt FROM Predictions WHERE prediction = 0 AND (results = 'Pass' OR results = 'Pass w/ Conditions')

		%%sql -q -o true_negative
		SELECT count(*) AS cnt FROM Predictions WHERE prediction = 1 AND results = 'Fail'

		%%sql -q -o false_negative
		SELECT count(*) AS cnt FROM Predictions WHERE prediction = 1 AND (results = 'Pass' OR results = 'Pass w/ Conditions') 

2. 最後に、次のスニペットを使用して、**Matplotlib** を使ってプロットを生成します。

		%%local
		%matplotlib inline
		import matplotlib.pyplot as plt
		
		labels = ['True positive', 'False positive', 'True negative', 'False negative']
		sizes = [true_positive['cnt'], false_positive['cnt'], false_negative['cnt'], true_negative['cnt']]
		colors = ['turquoise', 'seagreen', 'mediumslateblue', 'palegreen', 'coral']
		plt.pie(sizes, labels=labels, autopct='%1.1f%%', colors=colors)
		plt.axis('equal')
	
	次の出力が表示されます。
	
	![予測の出力](./media/hdinsight-apache-spark-machine-learning-mllib-ipython/output_26_1.png)


	このグラフでは、「positive」の結果は食品検査の不合格を指し、「negative」の結果は、食品検査の合格を指します。

## Notebook をシャットダウンする

アプリケーションの実行が完了したら、Notebook をシャットダウンしてリソースを解放する必要があります。そのためには、Notebook の **[ファイル]** メニューの **[閉じて停止]** をクリックします。これにより、Notebook がシャットダウンされ、閉じられます。


## <a name="seealso"></a>関連項目


* [概要: Azure HDInsight での Apache Spark](hdinsight-apache-spark-overview.md)

### シナリオ

* [Spark と BI: HDInsight と BI ツールで Spark を使用した対話型データ分析の実行](hdinsight-apache-spark-use-bi-tools.md)

* [Spark with Machine Learning: Use Spark in HDInsight for analyzing building temperature using HVAC data (Spark と Machine Learning: HDInsight で Spark を使用して HVAC データを基に建物の温度を分析する)](hdinsight-apache-spark-ipython-notebook-machine-learning.md)

* [Spark ストリーミング: リアルタイム ストリーミング アプリケーションを作成するための HDInsight での Spark の使用](hdinsight-apache-spark-eventhub-streaming.md)

* [Website log analysis using Spark in HDInsight (HDInsight での Spark を使用した Web サイト ログ分析)](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### アプリケーションの作成と実行

* [Scala を使用してスタンドアロン アプリケーションを作成する](hdinsight-apache-spark-create-standalone-application.md)

* [Livy を使用して Spark クラスターでジョブをリモートで実行する](hdinsight-apache-spark-livy-rest-interface.md)

### ツールと拡張機能

* [Use HDInsight Tools Plugin for IntelliJ IDEA to create and submit Spark Scala applicatons (Linux)](hdinsight-apache-spark-intellij-tool-plugin.md)

* [IntelliJ IDEA 用の HDInsight Tools プラグインを使用して Spark アプリケーションをリモートでデバッグする](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)

* [HDInsight の Spark クラスターで Zeppelin Notebook を使用する](hdinsight-apache-spark-use-zeppelin-notebook.md)

* [HDInsight 用の Spark クラスターの Jupyter Notebook で使用可能なカーネル](hdinsight-apache-spark-jupyter-notebook-kernels.md)

* [Jupyter Notebook で外部のパッケージを使用する](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)

* [Jupyter をコンピューターにインストールして HDInsight Spark クラスターに接続する](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### リソースの管理

* [Azure HDInsight での Apache Spark クラスターのリソースの管理](hdinsight-apache-spark-resource-manager.md)

* [HDInsight の Apache Spark クラスターで実行されるジョブの追跡とデバッグ](hdinsight-apache-spark-job-debugging.md)

<!---HONumber=AcomDC_0914_2016-->