<template>
  <div>
    <div class="menu-header" :class="{active: !culinaryExpand}" @click="expand">
      <div class="menu-title-holder" :class="{active: !culinaryExpand}" @click="expand">
        <p class="title-text">Culinary</p>
      </div>
      <div class="img-holder" :class="{active: !culinaryExpand}" @click="expand">
        <img :src="getImgUrl('Culinary.png')" class="my-card-img">
      </div>
    </div>
    <transition name="expand">
      <div v-if="culinaryExpand" class="culinary-holder menu-background">
        <b-button
          id="exPopoverReactive2"
          :disabled="foodPopoverShow"
          :variant="buttonFill"
          ref="button"
        >Select Culinary Preferences:</b-button>
        <div :hidden="!itemsSelected">Selected Culinary Prerences</div>
        <li class="listed-culinary" v-for="select in foodSelected" :key="select">{{select}}</li>
        <b-popover
          target="exPopoverReactive2"
          triggers="click"
          :show.sync="foodPopoverShow"
          placement="auto"
          container="myContainer"
          ref="popover"
        >
          <template slot="title">
            <b-button @click="onClose" class="close" aria-label="Close">
              <span class="d-inline-block" aria-hidden="true">&nbsp; &times; &nbsp;</span>
            </b-button>
            <strong>Culinary Selection</strong>
          </template>

          <div>
            <b-form-group label="Culinary Preferences">
              <b-form-checkbox-group
                switches
                v-model="foodSelected"
                stacked
                :options="foodOptions"
              />
            </b-form-group>
          </div>
        </b-popover>
      </div>
    </transition>
  </div>
</template>

<script>
// @ is an alias to /src
export default {
  name: "Team88CulinaryControls",
  components: {},
  props: {
    culinaryExpand: Boolean
  },
  data: function() {
    return {
      foodPopoverShow: false,
      foodSelected: [],
      itemsSelected: false,
      buttonFill: "outline-primary",
      expanded: false
    };
  },
  beforeMount() {
    this.$store.dispatch("getAllCuisine");
  },
  methods: {
    getImgUrl(pic) {
      return require(`../../assets/${pic}`);
    },
    onClose() {
      this.foodPopoverShow = false;
    },
    expand() {
      this.exapnded = this.culinaryExpand;
      this.$emit("expanded", [!this.expanded]);
    }
  },
  computed: {
    foodOptions() {
      return this.$store.state.culinaryOptions;
    }
  },
  watch: {
    foodSelected() {
      this.$store.dispatch("setSelectedCulinary", this.foodSelected);
      if (this.foodSelected.length > 0) {
        this.itemsSelected = true;
        this.buttonFill = "primary";
      } else {
        this.itemsSelected = false;
        this.buttonFill = "outline-primary";
      }
    }
  }
};
</script>

<style scoped lang='scss'>
@import "../../assets/stylesheets/ControlsMenus.scss";
.culinary-holder {
  padding: 4vh 0 4vh 0;
  overflow: auto;
}
.listed-culinary {
  text-align: left;
  margin-left: 20%;
}
</style>