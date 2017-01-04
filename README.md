# 資料視覺化相關工具的心得

## 工具列表
* [Tableau](http://www.tableau.com/)
* [Highcharts](http://www.highcharts.com/)
* [D3.js](https://d3js.org/)

## 心得

過去一年來，為了協助處理實驗室的計畫案，我學習了一些資料視覺化的工具：Tableau和Highcharts。剛好我有做這次知識挖掘的HW8，使用D3.js來繪製圖表，剛好可以來做個分享和比較。

先來講講Tableau，會用到Tableau，是因為某個計畫案原本就要求用這套商業軟體，將那個單位的統計資料繪製成圖表。確實Tableau還滿好用的，不用coding，程式就會將資料的標題模組化。將模組拉拉來拉去，再選擇適合的圖表樣式，一張好看又有互動效果的圖表就出來了。我覺得這套程式好用的地方在繪製地圖行圖表，只要有地名，程式基本上都能在地圖抓到對應的位置，然後就能快速完成一張地圖圖表。[這邊](http://ppt.cc/SSECj)是我後來為另外一個計畫案所畫圖表。不過Tableau的圖表樣式大約只有十幾種，實在不多。然而Tableau可以把多張圖表放在一起，並設定連動效果，也是另一種豐富畫面的效果吧。

後來某單位想要的效果(游標移到折線圖中間，會產生一條直線)Tableau畫不出來，於是實驗室的學長找了其他解決方法，就是用Highchart來產生那些效果。後來我也用了Highchart畫了一些[長條圖](http://ppt.cc/5Uckm)，Highchart是透過Javascript來實作(搭配jQuery)。程式碼的寫法相當簡單，官網上也有很詳盡的教學。大致上只要呼叫一個函式，並在裡面塞參數和資料，就能完成一張圖表，例如下方程式碼：

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

