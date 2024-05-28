## Amchart4

Using package :

```bash
flutter pub add fl_chart
```

Basic Usege :

- Create file **lib/screens/summary_screen.dart** 
```bash
import 'package:flutter/material.dart';
import 'package:ininoutout_flutter/widgets/bar%20graph/bar_graph.dart';

class SummaryScreen extends StatefulWidget {
  const SummaryScreen({super.key});

  @override
  State<SummaryScreen> createState() => _SummaryScreenState();
}

class _SummaryScreenState extends State<SummaryScreen> {
  List<double> weeklySummary = [4.44, 2.50, 43.42, 10.23, 23.50, 88.90, 13.60];

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: SafeArea(
        child: Center(
          child: SizedBox(
            height: 400,
            child: MyBarGraph(
              weeklySummary: weeklySummary,
            ),
          ),
        ),
      ),
    );
  }
}
```
- Create file **lib/widgets/bar graph/bar_data.dart** 
```bash
import 'package:ininoutout_flutter/widgets/bar%20graph/individual_bar.dart';

class BarData {
  final double sunAmount;
  final double monAmount;
  final double tueAmount;
  final double wedAmount;
  final double thuAmount;
  final double friAmount;
  final double satAmount;

  BarData({
    required this.sunAmount,
    required this.monAmount,
    required this.tueAmount,
    required this.wedAmount,
    required this.thuAmount,
    required this.friAmount,
    required this.satAmount,
  });

  List<IndividualBar> barData = [];

  // initialize bar data
  void initializeBarData() {
    barData = [
      IndividualBar(x: 0, y: sunAmount),
      IndividualBar(x: 0, y: monAmount),
      IndividualBar(x: 0, y: tueAmount),
      IndividualBar(x: 0, y: wedAmount),
      IndividualBar(x: 0, y: thuAmount),
      IndividualBar(x: 0, y: friAmount),
      IndividualBar(x: 0, y: satAmount),
    ];
  }
}
```
- Create file **lib/widgets/bar graph/bar_graph.dart** 
```bash
import 'package:flutter/material.dart';
import 'package:fl_chart/fl_chart.dart';
import 'package:ininoutout_flutter/widgets/bar%20graph/bar_data.dart';

class MyBarGraph extends StatelessWidget {
  final List weeklySummary;

  const MyBarGraph({
    super.key,
    required this.weeklySummary,
  });

  @override
  Widget build(BuildContext context) {
    BarData myBarData = BarData(
      sunAmount: weeklySummary[0],
      monAmount: weeklySummary[1],
      tueAmount: weeklySummary[2],
      wedAmount: weeklySummary[3],
      thuAmount: weeklySummary[4],
      friAmount: weeklySummary[5],
      satAmount: weeklySummary[6],
    );
    myBarData.initializeBarData();

    return BarChart(
      BarChartData(
        maxY: 200,
        minY: 0,
        barGroups: myBarData.barData
            .map(
              (data) => BarChartGroupData(
                x: data.x,
                barRods: [
                  BarChartRodData(toY: data.y),
                ],
              ),
            )
            .toList(),
      ),
    );
  }
}
```
- Create file **lib/widgets/bar graph/individual_bar.dart** 
```bash
class IndividualBar {
  final int x;
  final double y;

  IndividualBar({
    required this.x,
    required this.y,
  });
}
```
- Edit fild **lib/main.dart**
```bash
import 'package:flutter/material.dart';
import 'package:ininoutout_flutter/constants.dart';
import 'package:ininoutout_flutter/screens/main_screeen.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'My Flutter App',
      theme: ThemeData(
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.deepPurple),
        useMaterial3: true,
        ),
      ),
      home: const SummaryScreen(),
    );
  }
}

```
Result :
![result](https://github.com/Pratchaya0/webapp-knowledge/blob/main/src/fl_chart_ex1.png)
