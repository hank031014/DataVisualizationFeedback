# 資料視覺化相關工具的心得

## 工具列表
* [Tableau](http://www.tableau.com/)
* [Highcharts](http://www.highcharts.com/)
* [D3.js](https://d3js.org/)

## 心得

過去一年來，為了協助處理實驗室的計畫案，我學習了一些資料視覺化的工具：Tableau和Highcharts。剛好我有做這次知識挖掘的HW8，使用D3.js來繪製圖表，剛好可以來做個分享和比較。

先來講講Tableau，會用到Tableau，是因為某個計畫案原本就要求用這套商業軟體，將那個單位的統計資料繪製成圖表。確實Tableau還滿好用的，不用coding，程式就會將資料的標題模組化。將模組拉拉來拉去，再選擇適合的圖表樣式，一張好看又有互動效果的圖表就出來了。我覺得這套程式好用的地方在繪製地圖行圖表，只要有地名，程式基本上都能在地圖抓到對應的位置，然後就能快速完成一張地圖圖表。[這邊](http://ppt.cc/SSECj)是我後來為另外一個計畫案所畫圖表。不過Tableau的圖表樣式大約只有十幾種，實在不多。然而Tableau可以把多張圖表放在一起，並設定連動效果，也是另一種豐富畫面的效果吧。

後來某單位想要的效果(游標移到折線圖中間，會產生一條直線)Tableau畫不出來，於是實驗室的學長找了其他解決方法，就是用Highchart來產生那些效果。後來我也用了Highchart畫了一些[長條圖](http://ppt.cc/5Uckm)，Highchart是透過Javascript來實作(搭配jQuery)。程式碼的寫法相當簡單，官網上也有很詳盡的教學。大致上只要呼叫一個函式，並在裡面塞參數和資料，就能完成一張圖表。Highchart提供了大概數十種圖表樣式，應用實作也算是容易。如果是資料視覺化的初心者，我個人還滿建議使用Highchart來實作。

最近為了交這門課的作業，而選擇做HW8([成品在此](https://hank031014.github.io/gov_loan_data/))。由於課程PPT裡強烈建議使用D3，於是我就使用D3來畫圖表。進入D3官網，真是令人印象深刻，裡面羅列大約近百種圖表樣式。有些外觀真的非常特別，有些則有很棒的互動效果。不過我最後使用多線條這線圖來做而已，一來是不太熟悉D3的寫法，二來是各級政府的負債資料其實用折線圖就能將訊息表達得很清楚。相較於Highchart，D3的程式寫法真的麻煩許多，但圖表樣式卻相對多很多。

接下來就做個Highchart和D3的比較，下方是Highchart的程式碼：
	Highcharts.chart('container', {
        title: {
            text: 'Monthly Average Temperature',
            x: -20 //center
        },
        subtitle: {
            text: 'Source: WorldClimate.com',
            x: -20
        },
        xAxis: {
            categories: ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun',
                'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec']
        },
        yAxis: {
            title: {
                text: 'Temperature (°C)'
            },
            plotLines: [{
                value: 0,
                width: 1,
                color: '#808080'
            }]
        },
        tooltip: {
            valueSuffix: '°C'
        },
        legend: {
            layout: 'vertical',
            align: 'right',
            verticalAlign: 'middle',
            borderWidth: 0
        },
        series: [{
            name: 'Tokyo',
            data: [7.0, 6.9, 9.5, 14.5, 18.2, 21.5, 25.2, 26.5, 23.3, 18.3, 13.9, 9.6]
        }, {
            name: 'New York',
            data: [-0.2, 0.8, 5.7, 11.3, 17.0, 22.0, 24.8, 24.1, 20.1, 14.1, 8.6, 2.5]
        }, {
            name: 'Berlin',
            data: [-0.9, 0.6, 3.5, 8.4, 13.5, 17.0, 18.6, 17.9, 14.3, 9.0, 3.9, 1.0]
        }, {
            name: 'London',
            data: [3.9, 4.2, 5.7, 8.5, 11.9, 15.2, 17.0, 16.6, 14.2, 10.3, 6.6, 4.8]
        }]
    });
這份程式碼取自Highchart官網的[Demo圖表](http://www.highcharts.com/demo/line-basic)，呼叫了chart()後，在裡面填入圖表各式各樣的元素，例如XY軸、提示框、標題、顏色，還有最重要的數據資料，填入格式很像JSON。接著放到瀏覽器上運行就能看見圖表。接著是我為了HW8寫的D3程式碼：
	var div = d3.select("body").append("div")	
    .attr("class", "tooltip")				
    .style("opacity", 0);
	
	var svg = d3.select("#svg1").append("svg:svg").attr("height", 500).attr("width", 960),
    margin = {top: 20, right: 70, bottom: 30, left: 70},
    width = +svg.attr("width") - margin.left - margin.right,
    height = +svg.attr("height") - margin.top - margin.bottom,
    g = svg.append("g").attr("transform", "translate(" + margin.left + "," + margin.top + ")");
	
	var x = d3.scaleLinear().range([0, width]),
    y = d3.scaleLinear().range([height, 0]),
    z = d3.scaleOrdinal(d3.schemeCategory10);
	
	var line = d3.line()
    .x(function(d) { return x(d.year); })
    .y(function(d) { return y(d.total); });
	var line2 = d3.line()
    .x(function(d) { return x2(d.year); })
    .y(function(d) { return y2(d.total); });

	var callback = function(error, data) {
		if (error) throw error;
		
		var governs = d3.nest()
		.key(function(d) { return d.government; })
		.entries(data);
	
		var clgovern = [], lkgovern = [];
		for(var i = 0; i < governs.length; i++){
			if(governs[i].key == "中央政府" || governs[i].key == "地方政府"){				
				clgovern.push(governs[i]);
				var v = governs[i].values;				
			}
			if(lk.find(findLkgovern, governs[i].key)){
				lkgovern.push(governs[i]);
			}
		}
		
		x.domain([
			d3.min(governs, function(c) { return d3.min(c.values, function(d) { return d.year - 1; }); }),
			d3.max(governs, function(c) { return d3.max(c.values, function(d) { return d.year + 1; }); })
		]);
		y.domain([
			d3.min([0]),
			d3.max(clgovern, function(c) { return d3.max(c.values, function(d) { return d.total; }); })
		]);
		z.domain(governs.map(function(c) { return c.government; }));
	
		g.append("g")
		.attr("class", "axis axis--x")
		.attr("transform", "translate(0," + height + ")")
		.call(d3.axisBottom(x))
		.append("text")
		.attr("x", 840)
		.attr("dx", "0.71em")
		.attr("fill", "#000")
		.text("民國(年)");

		g.append("g")
		.attr("class", "axis axis--y")
		.call(d3.axisLeft(y))
		.append("text")
		.attr("x", 15)
		.attr("y", -10)
		.attr("dy", "0.71em")
		.attr("fill", "#000")
		.text("總負債(百萬元)");
		
		 var clgoverns = g.selectAll(".city")
			.data(clgovern)
			.enter().append("g")
			.attr("class", "city")
			.attr("id", function(d) { return d.key; });

		clgoverns.append("path")
		.attr("class", "line")
		.attr("d", function(d) { return line(d.values); })
		.style("stroke", function(d) { return z(d.key); });

		clgoverns.append("text")
		.datum(function(d) { return {key: d.key, value: d.values[d.values.length - 1]}; })
		.attr("transform", function(d) { return "translate(" + x(d.value.year) + "," + y(d.value.total) + ")"; })
		.attr("x", 3)
		.attr("dy", "0.35em")
		.style("font", "10px sans-serif")
		.text(function(d) { return d.key; });
		
		g.selectAll("dot")	
        .data(data)			
		.enter().append("circle")								
        .attr("r", 3)	
		.attr("fill", "steelblue")
        .attr("cx", function(d) { return x(d.year); })		 
        .attr("cy", function(d) { return y(d.total); })
		.attr("id", function(d) { return "svg1_" + d.government; })
        .on("mouseover", function(d) {	
			var str = "民國" + d.year + "年，"  + d.government + "<br>債務合計：" + d.total + " 百萬元";
			str += "<br>一年以上債務：" + d.over_one_year + " 百萬元";
			str += "<br>未滿一年債務：" + d.less_one_year + " 百萬元";
            div.transition()		
                .duration(200)		
                .style("opacity", .9);		
            div	.html(str)	
                .style("left", (d3.event.pageX) + "px")		
                .style("top", (d3.event.pageY - 28) + "px");	
            })					
        .on("mouseout", function(d) {		
            div.transition()		
                .duration(500)		
                .style("opacity", 0);	
        });
	};
	d3.request("loan.csv")
    .mimeType("text/csv; charset=big5")
    .response(function(xhr) { return d3.csvParse(xhr.responseText, row); })
    .get(callback);
圖表性質和上面的Highchart圖表一樣，都是多線條折線圖，但用D3來做就相對麻煩許多。圖表的每個細節都必須透過函式呼叫和給定數值來設定，提示框還要自行另外處理，似乎沒有內建的函式可以速成。