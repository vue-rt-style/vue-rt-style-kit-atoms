<script type="text/jsx">


  import {getSpacesClass, spacesParamsNames, spacesParamsProps} from "../../mixins/spaceClassMixin.js";
  import {getDisplayClass, displayParamsNames, displayParamsProps} from "../../mixins/displayClassMixin.js";

  const componentProps = {
    largeSize: {
      type: [Number, String],
      default: 0
    },
    size: {
      type: [Number, String],
      default: 0
    },
    tabletSize: {
      type: [Number, String],
      default: -1
    },
    mobileSize: {
      type: [Number, String],
      default: -1
    },
    hide: {
      type: Boolean,
      default: false
    },
    mHide: {
      type: Boolean,
      default: false
    },
    tHide: {
      type: Boolean,
      default: false
    },
    lgHide: {
      type: Boolean,
      default: false
    }
  }
  const componentsList = {};
  export default {
    name: "RtCol",
    props: {...componentProps, ...spacesParamsProps, ...displayParamsProps},
    data() {
      return {
        sizeParams: ['largeSize','size', 'mobileSize', 'tabletSize'],
      }
    },

    mounted() {
    },
    computed: {
      rowClassName() {
        const classNamesArray = [...spacesParamsNames.map((name) => {
          if (this[name] >= 0) {
            return getSpacesClass(name, this[name]);
          }
        }), ...displayParamsNames.map((name) => {
          if (this[name]) {
            return getDisplayClass(name, this[name]);
          }
        }), ...this.sizeParams.map((name) => {
          if (this[name] >= 0) {
            return this.getClassName(name, this[name])
          }
        })
        ].filter((i) => i && i.length > 0);
        if (this.hide || this.lgHide || this.tHide || this.mHide) {
          ['hide', 'lgHide', 'tHide', 'mHide'].forEach((key) => {
            let prefix = key.replace(/hide/gi, '');
            if (this[key]) {
              let hideClass = ['d', 'none']
              if(prefix == 'm' || prefix == 't'){
                prefix += 'd'
              }
	            if (prefix.length > 0) {
                hideClass.unshift(prefix)
              }

              classNamesArray.push(hideClass.join('-'))
            } else {
              let hideClass = ['d', 'block']
              if (prefix.length > 0) {
                hideClass.unshift(prefix)
              }
              classNamesArray.push(hideClass.join('-'))
            }
          })
        }

        return classNamesArray.join(' ');
      }
    },
    methods: {
      getClassName(name, value) {
        const classNameArray = ['rt', 'col'];
        if (value > 0) {
          switch (true) {
            case name.search(/large/i) >=0:
              classNameArray.push('lg');
              break;
            case name.search(/mobile/i) >= 0:
              classNameArray.push('md');
              break;
            case name.search(/tablet/i) >= 0:
              classNameArray.push('td');
              break;
          }
          classNameArray.push(value.toString());
        }
        return classNameArray.join('-');
      }
    },
    render(h) {
      return <div class={this.rowClassName}>{this.$slots.default}</div>
    }

  };
</script>
