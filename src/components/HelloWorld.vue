<template>
  <div class="component-container">
    <div class="builder-container">
      <div class="builder-content">
        <splitpanes
          class="main-splitpane"
          horizontal
          @resize="resizeRows"
          @resized="resizeDone"
        >
          <pane
            v-for="(row, index) in layout.rows"
            :key="index"
            min-size="5"
            class="row"
          >
            <div class="builder-layout-columns">
              <div>
              <i @click="changeColumnCount('add', index)" class="material-icons"
                >arrow_drop_down_circle</i
              >
              <p>{{ row.columns.length }}</p>
              <i
                @click="changeColumnCount('subtract', index)"
                class="material-icons"
                >arrow_drop_down_circle</i
              >
              <h1
                v-if="componentState.splitterIsHovered"
                class="pane-size-label-row"
              >
                {{ row.paneSize ? row.paneSize + "%" : null }}
              </h1>
              </div>
            </div>
            <splitpanes
              @resize="resizeColumns($event, index)"
              @resized="resizeDone"
            >
              <pane
                v-for="(column, index) in row.columns"
                :key="index"
                :class="
                  componentState.splitterIsHovered
                    ? 'column blue-background'
                    : 'column'
                "
                min-size="5"
              >
                <h1
                  v-if="componentState.splitterIsHovered"
                  class="pane-size-label-column"
                >
                  {{ column.paneSize ? column.paneSize + "%" : null }}
                </h1>
                <drop-zone v-else />
              </pane>
            </splitpanes>
          </pane>
        </splitpanes>
      </div>
    </div>
    <div class="builder-controls">
      <div class="builder-controls-layout">
        <div class="builder-layout-columns">
          <h5>Rows</h5>
          <i @click="changeRowCount('add')" class="material-icons"
            >arrow_drop_down_circle</i
          >
          <p>{{ layout.rows.length }}</p>
          <i @click="changeRowCount('subtract')" class="material-icons"
            >arrow_drop_down_circle</i
          >
        </div>
      </div>
      <div class="builder-controls-elements">
        <i
          v-for="(block, index) in buildingBlocks"
          @dragstart="drag($event, null)"
          :key="index"
          :draggable="true"
          class="drag-element material-icons"
        >
          {{ block.icon }}
        </i>
      </div>
    </div>
    <gif-picker-vue v-if="false" />
  </div>
</template>

<script>
import { Splitpanes, Pane } from "splitpanes";
import DropZone from "./DropZone.vue";
import "splitpanes/dist/splitpanes.css";
import GifPickerVue from "./gifPicker.vue";

export default {
  components: { Splitpanes, Pane, DropZone, GifPickerVue },
  name: "HelloWorld",
  data() {
    return {
      buildingBlocks: [
        { icon: "image", label: "Image" },
        { icon: "comment", label: "Text" },
        { icon: "font_download", label: "Text" },
        { icon: "gif", label: "GIF" },
        { icon: "insert_link", label: "Hyperlink" },
      ],
      componentState: {
        splitterIsHovered: true,
      },
      layout: {
        rows: [],
      },
    };
  },
  methods: {
    applyHoverActionToSplitters() {
      document
        .querySelectorAll(".splitpanes__splitter")
        .forEach((nodeElement) => {
          nodeElement.addEventListener("mouseenter", () => {
            this.componentState.splitterIsHovered = true;
          });
          nodeElement.addEventListener("mouseleave", () => {
            this.componentState.splitterIsHovered = false;
          });
        });
    },
    changeRowCount(operation) {
      if (operation === "add") {
        this.layout.rows.push({
          columns: [],
          paneSize: null,
        });
        this.layout.rows.forEach((row) => {
          row.paneSize = null;
        });
        setTimeout(() => {
          this.applyHoverActionToSplitters();
        }, 200);
      } else {
        this.layout.rows.pop();
      }
    },
    changeColumnCount(operation, rowIndex) {
      if (operation === "add") {
        this.layout.rows[rowIndex].columns.push({
          paneSize: null,
          columns: [],
        });
        this.layout.rows[rowIndex].columns.forEach((column) => {
          column.paneSize = null;
        });
        setTimeout(() => {
          this.applyHoverActionToSplitters();
        }, 200);
      } else {
        this.layout.rows[rowIndex].columns.pop();
      }
    },
    drag(e, type) {
      e.datatransfer.setData("text", type);
    },
    resizeRows(e) {
      e.forEach((item, index) => {
        this.layout.rows[index].paneSize = Math.round(item.size);
      });
      // this.layout.rows[]
    },
    resizeColumns(e, rowIndex) {
      e.forEach((item, index) => {
        this.layout.rows[rowIndex].columns[index].paneSize = Math.round(
          item.size
        );
      });
    },
    resizeDone(e) {
      console.log(e);
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="less">
.component-container {
  display: flex;
  flex-wrap: wrap;
  flex-direction: row;
  height: 60vh;
  margin-top: 40px;
  justify-content: center;
  .builder-controls {
    width: 70px;
    height: 100%;
    margin-left: 20px;
    display: flex;
    flex-direction: column;
    .builder-controls-layout {
      div {
        display: flex;
        flex-direction: column;
        justify-content: center;
        width: 100%;
      }
      margin-bottom: 30px;
    }

    .builder-controls-elements {
      width: 100%;
      height: 100%;
      display: flex;
      flex-direction: column;
      align-items: center;
      .drag-element {
        display: flex;
        justify-content: center;
        align-items: center;
        background-color: black;
        border-radius: 8px;
        width: 40px;
        height: 40px;
        margin-bottom: 10px;
        color: white;
        cursor: grab;
        -webkit-user-select: none;
        -moz-user-select: none;
        -ms-user-select: none;
        user-select: none;
      }
    }
  }
  .builder-layout-columns {
    width: 7%;
    height: 100%;
    display: flex;
    justify-content: center;
    align-items: center;

    h5 {
      margin: 0;
      margin-top: 10px;
    }

    i {
      margin: 0;
      font-size: 25px;
      cursor: pointer;

      &:first-of-type {
        transform: rotate(180deg);
      }
    }
    p {
      margin: 0;
    }
  }
  .builder-container {
    height: 100%;
    width: 80%;
    display: flex;
    .builder-content {
      min-width: 100%;
      border: 1px solid black;

      .pane-size-label-column {
        position: relative;
        top: 40%;
        font-size: 50px;
        z-index: 10;
        margin: 0;
      }

      .pane-size-label-row {
        position: relative;
        top: 10%;
        font-size: 44px;
        margin: 0;
      }
    }

    .row {
      display: flex;
    }
    .column:hover {
      border: 1px solid black;
      background-color: rgba(17, 90, 248, 0.3);
    }
    .blue-background {
      background-color: rgba(8, 40, 185, 0.068);
    }
  }

  //SPLITPANES STYLING
  .splitpanes {
    background-color: #f8f8f8 !important;
  }

  .splitpanes__splitter {
    background-color: #ccc !important;
    position: relative !important;
  }
  .splitpanes__splitter:before {
    content: "" !important;
    position: absolute !important;
    left: 0 !important;
    top: 0 !important;
    transition: opacity 0.4s !important;
    background-color: rgba(17, 90, 248, 0.3) !important;
    opacity: 0 !important;
    z-index: 1 !important;
  }
  .splitpanes__splitter:hover:before {
    opacity: 1 !important;
  }
  .splitpanes--vertical > .splitpanes__splitter:before {
    left: -10px !important;
    right: -10px !important;
    height: 100% !important;
  }
  .splitpanes--horizontal > .splitpanes__splitter:before {
    top: -10px !important;
    bottom: -10px !important;
    width: 100% !important;
  }
}
</style>