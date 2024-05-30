## Syncfusion flutter charts

Using package :

```bash
flutter pub add syncfusion_flutter_charts
```

Basic Usege :

- Update file **lib/main.dart** 
```bash
import 'package:flutter/material.dart';
import 'package:syncfusion_flutter_charts/charts.dart';
import 'package:syncfusion_flutter_charts/sparkcharts.dart';

void main() {
  return runApp(_ChartApp());
}

class _ChartApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      theme: ThemeData(primarySwatch: Colors.blue, useMaterial3: false),
      home: _MyHomePage(),
    );
  }
}

class _MyHomePage extends StatefulWidget {
  // ignore: prefer_const_constructors_in_immutables
  _MyHomePage({Key? key}) : super(key: key);

  @override
  _MyHomePageState createState() => _MyHomePageState();
}

class _MyHomePageState extends State<_MyHomePage> {
  List<_SalesData> data = [
    _SalesData('Jan', 35),
    _SalesData('Feb', 28),
    _SalesData('Mar', 34),
    _SalesData('Apr', 32),
    _SalesData('May', 40)
  ];
  @override
  Widget build(BuildContext context) {
    return Scaffold(
        appBar: AppBar(
          title: const Text('Syncfusion Flutter chart'),
        ),
        body: Column(children: [
          //Initialize the chart widget
          SfCartesianChart(
              primaryXAxis: CategoryAxis(),
              // Chart title
              title: ChartTitle(text: 'Half yearly sales analysis'),
              // Enable legend
              legend: Legend(isVisible: true),
              // Enable tooltip
              tooltipBehavior: TooltipBehavior(enable: true),
              series: <CartesianSeries<_SalesData, String>>[
                LineSeries<_SalesData, String>(
                    dataSource: data,
                    xValueMapper: (_SalesData sales, _) => sales.year,
                    yValueMapper: (_SalesData sales, _) => sales.sales,
                    name: 'Sales',
                    // Enable data label
                    dataLabelSettings: DataLabelSettings(isVisible: true))
              ]),
          Expanded(
            child: Padding(
              padding: const EdgeInsets.all(8.0),
              //Initialize the spark charts widget
              child: SfSparkLineChart.custom(
                //Enable the trackball
                trackball: SparkChartTrackball(
                    activationMode: SparkChartActivationMode.tap),
                //Enable marker
                marker: SparkChartMarker(
                    displayMode: SparkChartMarkerDisplayMode.all),
                //Enable data label
                labelDisplayMode: SparkChartLabelDisplayMode.all,
                xValueMapper: (int index) => data[index].year,
                yValueMapper: (int index) => data[index].sales,
                dataCount: 5,
              ),
            ),
          )
        ]));
  }
}

class _SalesData {
  _SalesData(this.year, this.sales);

  final String year;
  final double sales;
}
```

Example 2:
Bar chart group green and red
```bash
import 'package:flutter/material.dart';
import 'package:ininoutout_flutter/constants.dart'; ***option
import 'package:syncfusion_flutter_charts/charts.dart';
import 'package:syncfusion_flutter_charts/sparkcharts.dart';

class SummaryScreen extends StatefulWidget {
  const SummaryScreen({super.key});

  @override
  State<SummaryScreen> createState() => _SummaryScreenState();
}

class _SummaryScreenState extends State<SummaryScreen> {
  String selectedChips = 'Dec';
  List<String> chips = [
    'Nov',
    'Dec',
    'Jan',
    'Feb',
    'Mar',
    'Apr',
  ];

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: SafeArea(
        child: Card(
          color: Colors.white,
          surfaceTintColor: Colors.white,
          shape: RoundedRectangleBorder(
            borderRadius: BorderRadius.circular(20),
          ),
          child: Column(
            children: [
              Container(
                padding: const EdgeInsets.symmetric(horizontal: 5, vertical: 5),
                child: Column(
                  children: [
                    FittedBox(
                      child: Wrap(
                        spacing: 10,
                        children: chips.map((category) {
                          return ChoiceChip(
                            label: Text(category),
                            labelStyle: const TextStyle(color: Colors.black),
                            selectedColor: Colors.grey.shade200,
                            backgroundColor: Colors.white,
                            showCheckmark: false,
                            selected: selectedChips.contains(category),
                            side:
                                const BorderSide(width: 0, color: Colors.white),
                            shape: RoundedRectangleBorder(
                              borderRadius: BorderRadius.circular(10),
                            ),
                            onSelected: (isSelected) {
                              setState(() {
                                if (isSelected) {
                                  selectedChips = category;
                                }
                              });
                            },
                          );
                        }).toList(),
                      ),
                    ),
                    SfCartesianChart(
                      margin: EdgeInsets.symmetric(vertical: 10),
                      borderWidth: 0,
                      primaryXAxis: const CategoryAxis(
                        isVisible: false,
                      ),
                      primaryYAxis: const NumericAxis(
                        isVisible: false,
                        minimum: 0,
                        maximum: 2,
                        interval: 0.5,
                      ),
                      series: <CartesianSeries>[
                        ColumnSeries<ChartColumnData, String>(
                          dataSource: chartData,
                          width: 0.5,
                          color: Colors.green,
                          xValueMapper: (ChartColumnData data, _) => data.x,
                          yValueMapper: (ChartColumnData data, _) => data.y1,
                        ),
                        ColumnSeries<ChartColumnData, String>(
                          dataSource: chartData,
                          width: 0.5,
                          color: Colors.red,
                          xValueMapper: (ChartColumnData data, _) => data.x,
                          yValueMapper: (ChartColumnData data, _) => data.y2,
                        ),
                      ],
                    ),
                    Row(
                      children: [
                        Container(
                          width: 27,
                          height: 13,
                          decoration: BoxDecoration(
                              color: Colors.green,
                              borderRadius: BorderRadius.circular(20)),
                        ),
                        const SizedBox(
                          width: 10,
                        ),
                        const Text(
                          'Apple',
                          style: TextStyle(
                            fontSize: 14,
                            fontWeight: FontWeight.w500,
                            color: Colors.black,
                          ),
                        ),
                        Container(
                          width: 27,
                          height: 13,
                          decoration: BoxDecoration(
                              color: Colors.red,
                              borderRadius: BorderRadius.circular(20)),
                        ),
                        const SizedBox(
                          width: 10,
                        ),
                        const Text(
                          'Apple',
                          style: TextStyle(
                            fontSize: 14,
                            fontWeight: FontWeight.w500,
                            color: Colors.black,
                          ),
                        ),
                      ],
                    )
                  ],
                ),
              ),
              Container(
                padding: const EdgeInsets.all(5),
                decoration: BoxDecoration(
                  color: Colors.grey.shade200,
                  borderRadius: const BorderRadius.only(
                    bottomLeft: Radius.circular(20),
                    bottomRight: Radius.circular(20),
                  ),
                ),
                child: Row(
                  children: [
                    Container(
                      padding: const EdgeInsets.all(5),
                      decoration: BoxDecoration(
                        shape: BoxShape.circle,
                        color: Colors.white,
                        border: Border.all(
                          width: 3,
                          color: Colors.purple,
                        ),
                      ),
                    )
                  ],
                ),
              )
            ],
          ),
        ),
      ),
    );
  }
}

class ChartColumnData {
  ChartColumnData(this.x, this.y1, this.y2);
  final String x;
  final double? y1;
  final double? y2;
}

final List<ChartColumnData> chartData = <ChartColumnData>[
  ChartColumnData("Su", 1.7, 1),
  ChartColumnData("Mo", 1.3, 1.7),
  ChartColumnData("Tu", 0.8, 1.9),
  ChartColumnData("We", 1.5, 1.5),
  ChartColumnData("Th", 1.9, 0.8),
  ChartColumnData("Fr", 0.5, 1.5),
  ChartColumnData("Sa", 2, 0.5),
];
```