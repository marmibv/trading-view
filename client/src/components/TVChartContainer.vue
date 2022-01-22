<template>
  <div class="TVChartContainer" :id="container" />
</template>

<script>
import api from "./api";

export default {
  name: "TVChartContainer",
  props: {
    symbol: {
      default: "AAPL",
      type: String,
    },
    interval: {
      default: "D",
      type: String,
    },
    container: {
      default: "tv_chart_container",
      type: String,
    },
    datafeedUrl: {
      default: "https://demo_feed.tradingview.com",
      type: String,
    },
    libraryPath: {
      default: "/charting_library/charting_library/",
      type: String,
    },
    chartsStorageUrl: {
      default: "https://saveload.tradingview.com",
      type: String,
    },
    chartsStorageApiVersion: {
      default: "1.2",
      type: String,
    },
    clientId: {
      default: "tradingview.com",
      type: String,
    },
    userId: {
      default: "public_user_id",
      type: String,
    },
    fullscreen: {
      default: true,
      type: Boolean,
    },
    autosize: {
      default: true,
      type: Boolean,
    },
    studiesOverrides: {
      type: Object,
    },
  },
  tvWidget: null,
  mounted() {
  if(window.location.toString().split('/').pop().toUpperCase() != "")
    this.symbol=window.location.toString().split('/').pop().toUpperCase()+"USDT";
      const widgetOptions = {
      symbol: this.symbol,
      // BEWARE: no trailing slash is expected in feed URL
      datafeed: api, // new window.Datafeeds.UDFCompatibleDatafeed(this.datafeedUrl),
      interval: this.interval,
      container: this.container,
      library_path: this.libraryPath,

      locale: "en",
      disabled_features: ["use_localstorage_for_settings"],
      // enabled_features: ['study_templates'],
      charts_storage_url: this.chartsStorageUrl,
      charts_storage_api_version: this.chartsStorageApiVersion,
      client_id: this.clientId,
      user_id: this.userId,
      fullscreen: this.fullscreen,
      autosize: this.autosize,
      studies_overrides: this.studiesOverrides,
      timezone: Intl.DateTimeFormat().resolvedOptions().timeZone,
      theme: "dark",
      disabled_features: [
        "save_chart_properties_to_local_storage",
        "volume_force_overlay",
        "trading_account_manager",
        "show_object_tree"
      ]
    };

    this.tvWidget = new TradingView.widget(widgetOptions);

    this.tvWidget.onChartReady(() => {
      /*
        tvWidget.headerReady().then(() => {
          const button = tvWidget.createButton();

          button.setAttribute('title', 'Click to show a notification popup');
          button.classList.add('apply-common-tooltip');

          button.addEventListener('click', () => tvWidget.showNoticeDialog({
            title: 'Notification',
            body: 'TradingView Charting Library API works correctly',
            callback: () => {
              // eslint-disable-next-line no-console
              console.log('Noticed!');
            },
          }));

          button.innerHTML = 'Check API';
        });

*/
    });
  },
  destroyed() {
    if (this.tvWidget !== null) {
      this.tvWidget.remove();
      this.tvWidget = null;
    }
  },
};
</script>

<style>
html{
  overflow:hidden;
}
.TVChartContainer {
  position: absolute;
  width: 102.5vw;
  height: 100%;
  overflow:hidden;
}

</style>
