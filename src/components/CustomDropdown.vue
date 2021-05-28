<template>
  <div class="" v-clickoutside="onClose">
    <!-- button -->
    <button class="btn-main-class" @click="saveLocations">
      <slot v-if="hasBtnText" name="btnText"></slot>
      <template v-else>{{ placeholder }}</template>
    </button>

    <!-- select body -->
    <div v-if="popupLocationFilter" class="bg">
      <!-- search input -->
      <div class="w-full" v-if="searchable">
        <input
          v-model="stateToSearch"
          class=""
          :placeholder="searchPlaceholder"
          @input="handleTyping()"
        />
      </div>

      <!-- options container -->
      <div class="">
        <div
          :key="index"
          v-for="(item, index) in elements"
          class="pointer"
          :class="{ hide: !item.matches }"
        >
          <p
            v-if="isChecked"
            :class="
              `flex pointer ${item.checked ? itemClass : ''} ${item.disabled ? 'disabled' : ''}`
            "
          >
            <input
              class="pointer"
              type="checkbox"
              :id="item.name"
              :name="item.name"
              :value="item.name"
              :checked="item.checked && !item.disabled"
              :disabled="item.disabled"
              @click="handleSelection(item)"
            />
            <label :for="item.name" class="pointer">{{ item.name }}</label>
          </p>
          <div v-else>
            <p
              @click="handleSelection(item)"
              :for="item.name"
              :class="
              `flex pointer ${item.checked ? itemClass : ''} ${item.disabled ? 'disabled' : ''}`
            "
            >
              {{ item.name }}
            </p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "DropdownCustomize",
  props: {
    items: {
      type: Array,
      default: () => []
    },
    multiSelect: {
      type: Boolean,
      default: false
    },
    searchable: {
      type: Boolean,
      default: false
    },
    isChecked: {
      type: Boolean,
      default: false
    },
    placeholder: {
      type: String,
      default: "Select an Option"
    },
    itemClass: {
      type: String,
      default: ""
    },
    searchPlaceholder: {
      type: String,
      default: ""
    }
  },
  data() {
    return {
      stateToSearch: "",
      popupLocationFilter: false,
      selectedToExport: [],
      elements: []
    };
  },
  computed: {
    hasBtnText() {
      return !!this.$slots.btnText;
    },
    itemClassName() {
      if (this.itemClass) {
        return this.itemClass;
      }
      return "selected";
    }
  },
  created() {
    this.formatItems();
  },
  methods: {
    onClose() {
      this.saveLocations();
      this.popupLocationFilter = false;
    },
    clearFilters() {
      this.elements.forEach((item) => {
        item.checked = false;
        item.matches = true;
      });

      this.stateToSearch = "";
    },
    handleTyping() {
      this.elements.forEach((item) => {
        let matchWithStateName = item.name
          .toLowerCase()
          .includes(this.stateToSearch.toLowerCase());

        item.matches = matchWithStateName ? true : false;
      });
    },
    formatItems() {
      this.elements = this.items;
      this.elements.forEach((item) => {
        item.matches = true;
        if (item.checked) {
          this.selectedToExport.push(item.value);
        } else {
          item.checked = false;
        }
        if (!item.disabled) {
          item.disabled = false;
        }
      });
    },
    handleSelection(item) {
      if (item.checked) {
        item.checked = false;
        if (Array.isArray(this.selectedToExport)) {
          this.selectedToExport = this.selectedToExport.filter(
            (e) => e != item.value
          );
        }
      } else {
        if (!item.disabled) {
          if (this.multiSelect) {
            this.selectedToExport.push(item.value);
          } else {
            this.selectedToExport = item.value;
            this.elements.map((i) => {
              i.checked = false;
            });
          }
          item.checked = true;
        }
      }
      this.$forceUpdate();
    },
    saveLocations() {
      this.popupLocationFilter = !this.popupLocationFilter;
      this.$emit("elementsSelected", this.selectedToExport);
      if (!this.popupLocationFilter) {
        this.$emit("hidden");
      }
    },
    cleanSelected(name) {
      this.selectedToExport = this.selectedToExport.filter((e) => e != name);
      name = name.split(" (")[0];
    }
  },
  directives: {
    clickoutside: {
      bind: function(el, binding, node) {
        el.clickOutsideEvent = function(event) {
          if (!(el == event.target || el.contains(event.target))) {
            node.context[binding.expression](event);
          }
        };
        document.body.addEventListener("click", el.clickOutsideEvent);
      },
      unbind: function(el) {
        document.body.removeEventListener("click", el.clickOutsideEvent);
      },
      stopProp(event) {
        event.stopPropagation();
      }
    }
  }
};
</script>

<style lang="css" scoped>
.btn-main-class {
  position: relative;
}
.bg {
  position: absolute;
  border: 1px solid #ccc;
  min-width: 25%;
  z-index: 1000;
  background-color: #fff;
  border: 1px solid #ccc;
  border-radius: 4px;
  box-shadow: 0 6px 12px rgb(0 0 0 / 18%);
  background-clip: padding-box;
}
.flex {
  display: flex;
}
.hide {
  display: none !important;
}
.pointer {
  cursor: pointer;
}
.selected {
  background: #41b883;
}
.disabled {
  /* color: red; */
  color: #ccc;
}
p {
  margin: 0;
  padding: 0.5em;
}
label {
  margin-left: 0.75em;
}
</style>
