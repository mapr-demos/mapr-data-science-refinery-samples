# Visualization in MapR Data Science Refinery
Apache Zeppelin supports the Helium framework. Using visualization packages, you can view your data through area charts, bar charts, scatter charts, and other displays. To use a visualization package, you must enable it through the Helium repository browser in the Zeppelin UI. 
Like [Zeppelin interpreters](doc/tutorials/002-interpreters.md), Helium is automatically installed in your Zeppelin container.

> **Important:** The Apache Community provides and supports the visualization packages available through the Helium repository browser. MapR does not provide support for these packages.

Follow these steps to enable a package:
1. Open the Helium repository browser by selecting the **Helium** tab in the main menu of the Zeppelin UI:
![Zeppelin vizualisation](doc/tutorials/images/zeppelin-visualization-1.png)
2. Locate your package, click **Enable**, and then click **OK** in the popup window:
![Zeppelin vizualisation](doc/tutorials/images/zeppelin-visualization-2.png)
> The time it takes to enable a package depends on your internet connection speed.
3. After enabling the package and refreshing your browser to reload notebook content, you can use the package. The following shows output that uses the `ultimate-pie-chart` package:
![Zeppelin vizualisation](doc/tutorials/images/zeppelin-visualization-3.png)

Currently, Helium supports 4 types of package.

[Helium Visualization(https://zeppelin.apache.org/docs/0.8.1/development/helium/writing_visualization_basic.html): Adding a new chart type
[Helium Spell](https://zeppelin.apache.org/docs/0.8.1/development/helium/writing_spell.html): Adding new interpreter, display system running on browser
[Helium Application](https://zeppelin.apache.org/docs/0.8.1/development/helium/writing_application.html)
[Helium Interpreter](https://zeppelin.apache.org/docs/0.8.1/development/writing_zeppelin_interpreter.html): Adding a new custom interpreter