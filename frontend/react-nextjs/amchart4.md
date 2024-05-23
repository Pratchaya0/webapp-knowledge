## Edge store

Using package :

```bash
npm install @amcharts/amcharts4
```

Basic Usege :

- Create folder **app/amchart/page.ts** 
```bash
import ExampleOne from "@/components/amchart/example-one";
import ExampleThree from "@/components/amchart/example-three";
import ExampleTwo from "@/components/amchart/example-two";

const AmchartPage = () => {
  return ( 
    <div>
        <ExampleThree />
    </div>
  );
};

export default AmchartPage;
```

- Create folder **app/component/amchart/example-three.ts** 

```bash
"use client";

import React, { Component } from "react";
import * as am4core from "@amcharts/amcharts4/core";
import * as am4charts from "@amcharts/amcharts4/charts";
import am4themes_animated from "@amcharts/amcharts4/themes/animated";

am4core.useTheme(am4themes_animated);

class ExampleThree extends Component {
  componentDidMount() {
    let chart = am4core.create("exampleThree", am4charts.XYChart);

    chart.data = [
      {
        country: "USA",
        visits: 2025,
        bullet:
          "https://firebasestorage.googleapis.com/v0/b/learn-firebase-768a9.appspot.com/o/user-profile%2F1%20(1).jpg?alt=media&token=1aea8fdc-e5bb-4812-9c34-428ce53892a5",
      },
      {
        country: "China",
        visits: 1882,
        bullet:
          "https://firebasestorage.googleapis.com/v0/b/learn-firebase-768a9.appspot.com/o/user-profile%2F1%20(1).jpg?alt=media&token=1aea8fdc-e5bb-4812-9c34-428ce53892a5",
      },
      {
        country: "Japan",
        visits: 1809,
        bullet:
          "https://firebasestorage.googleapis.com/v0/b/learn-firebase-768a9.appspot.com/o/user-profile%2F1%20(1).jpg?alt=media&token=1aea8fdc-e5bb-4812-9c34-428ce53892a5",
      },
      {
        country: "Germany",
        visits: 1322,
        bullet:
          "https://firebasestorage.googleapis.com/v0/b/learn-firebase-768a9.appspot.com/o/user-profile%2F1%20(1).jpg?alt=media&token=1aea8fdc-e5bb-4812-9c34-428ce53892a5",
      },
      {
        country: "UK",
        visits: 1122,
        bullet:
          "https://firebasestorage.googleapis.com/v0/b/learn-firebase-768a9.appspot.com/o/user-profile%2F1%20(1).jpg?alt=media&token=1aea8fdc-e5bb-4812-9c34-428ce53892a5",
      },
      {
        country: "France",
        visits: 1114,
        bullet:
          "https://firebasestorage.googleapis.com/v0/b/learn-firebase-768a9.appspot.com/o/user-profile%2F1%20(1).jpg?alt=media&token=1aea8fdc-e5bb-4812-9c34-428ce53892a5",
      },
    ];

    chart.padding(40, 40, 40, 40);

    let categoryAxis = chart.xAxes.push(new am4charts.CategoryAxis());
    categoryAxis.renderer.grid.template.location = 0;
    categoryAxis.dataFields.category = "country";
    categoryAxis.renderer.minGridDistance = 60;
    categoryAxis.renderer.inversed = true;
    categoryAxis.renderer.grid.template.disabled = true;

    let valueAxis = chart.yAxes.push(new am4charts.ValueAxis());
    valueAxis.min = 0;
    valueAxis.extraMax = 0.1;
    //valueAxis.rangeChangeEasing = am4core.ease.linear;
    //valueAxis.rangeChangeDuration = 1500;

    let series = chart.series.push(new am4charts.ColumnSeries());
    series.dataFields.categoryX = "country";
    series.dataFields.valueY = "visits";
    series.tooltipText = "{valueY.value}";
    series.columns.template.strokeOpacity = 0;
    series.columns.template.column.cornerRadiusTopRight = 10;
    series.columns.template.column.cornerRadiusTopLeft = 10;
    //series.interpolationDuration = 1500;
    //series.interpolationEasing = am4core.ease.linear;

    // let labelBullet = series.bullets.push(new am4charts.LabelBullet());
    // labelBullet.label.verticalCenter = "bottom";
    // labelBullet.label.dy = -10;
    // labelBullet.label.text = "{values.valueY.workingValue.formatNumber('#.')}";
    // Add bullets
    let bullet = series.bullets.push(new am4charts.Bullet());
    let image = bullet.createChild(am4core.Image);
    image.horizontalCenter = "middle";
    image.verticalCenter = "bottom";
    image.dy = 20;
    image.y = am4core.percent(100);
    image.propertyFields.href = "bullet";
    image.tooltipText = series.columns.template.tooltipText;
    image.propertyFields.fill = "color";
    image.filters.push(new am4core.DropShadowFilter());

    chart.zoomOutButton.disabled = true;

    // as by default columns of the same series are of the same color, we add adapter which takes colors from chart.colors color set
    series.columns.template.adapter.add("fill", function (fill, target) {
      if (target.dataItem) {
        return chart.colors.getIndex(target.dataItem.index);
      }
    });

    setInterval(function () {
      am4core.array.each(chart.data, function (item) {
        item.visits += Math.round(Math.random() * 200 - 100);
        item.visits = Math.abs(item.visits);
      });
      chart.invalidateRawData();
    }, 2000);

    categoryAxis.sortBySeries = series;
  }

  componentWillUnmount() {
    let _chart = am4core.create("exampleThree", am4charts.XYChart);
    if (_chart) {
      _chart.dispose();
    }
  }

  render() {
    return (
      <div id="exampleThree" style={{ width: "100%", height: "500px" }}></div>
    );
  }
}

export default ExampleThree;
```

Base structure :

```bash
"use client";

import React, { Component } from "react";
import * as am4core from "@amcharts/amcharts4/core";
import * as am4charts from "@amcharts/amcharts4/charts";
import am4themes_animated from "@amcharts/amcharts4/themes/animated";

am4core.useTheme(am4themes_animated);

class ExampleThree extends Component {
  componentDidMount() {
   // From  let chart = am4core.create("chartdiv", ... 

   ...
   
   // TO the end
  }

  componentWillUnmount() {
    let _chart = am4core.create("exampleThree", am4charts.XYChart);
    if (_chart) {
      _chart.dispose();
    }
  }

  render() {
    return (
      <div id="exampleThree" style={{ width: "100%", height: "500px" }}></div>
    );
  }
}

export default ExampleThree;
```


[Config](https://www.amcharts.com/docs/v4/getting-started/integrations/using-react/)

[Demos](https://www.amcharts.com/demos-v4/) -> *Example Chart and code