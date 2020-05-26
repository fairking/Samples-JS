<template>
  <div id="app">
    <div>
      <h2>Stimulsoft Reports.JS Designer</h2>
      <div id="designer"></div>
    </div>
  </div>
</template>

<script>
export default {
  name: "app",
  data() {
    return {};
  },
  mounted: function() {
    console.log("Loading Designer view");

    console.log("Set full screen mode for the designer");
    var options = new window.Stimulsoft.Designer.StiDesignerOptions();
    options.appearance.fullScreenMode = false;

    console.log("Create the report designer with specified options");
    var designer = new window.Stimulsoft.Designer.StiDesigner(
      options,
      "StiDesigner",
      false
    );

    designer.onBeginProcessData = this.onBeginProcessData;

    console.log("Create a new report instance");
    var report = new window.Stimulsoft.Report.StiReport();

    console.log("Load report from url");
    report.loadFile("reports/SimpleList.mrt");

    // https://services.odata.org/TripPinRESTierService/(S(vuzbl01wsrehumusahsgyqbl))/
    // https://services.odata.org/TripPinRESTierService/(S(vuzbl01wsrehumusahsgyqbl))/People('scottketchum')/Trips
    var db = new window.Stimulsoft.Report.Dictionary.StiODataDatabase(
      "OpenData",
      "odata",
      "https://services.odata.org/TripPinRESTierService/(S(vuzbl01wsrehumusahsgyqbl))/",
      false
    );
    report.dictionary.databases.add(db);

    report.onBeginProcessData = this.onBeginProcessData;

    console.log("Edit report template in the designer");
    designer.report = report;

    console.log("Rendering the viewer to selected element");
    designer.renderHtml("designer");

    console.log("Loading completed successfully!");
  },
  methods: {
    onBeginProcessData(e) {
      e.headers.push({ key: "Authorization", value: "Bearer 123456789" });
      console.log(e);
    }
  }
};
</script>

<style>
</style>
