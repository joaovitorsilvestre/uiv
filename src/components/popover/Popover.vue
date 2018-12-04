<script>
  import {TRIGGERS} from '../../utils/domUtils'
  import popupMixin from '../../mixins/popupMixin'

  export default {
    mixins: [popupMixin],
    data () {
      return {
        name: 'popover'
      }
    },
    async render (h) {
      function isFunction(functionToCheck) {
        return functionToCheck && {}.toString.call(functionToCheck) === '[object Function]';
      }

      let content
      if (isFunction(this.content)) {
        content = await this.content
      } else {
        content = this.content
      }

      const domProps = this.html === true ? {innerHTML: content} : {}

      return h(this.tag,
        [
          this.$slots.default,
          h('div',
            {
              style: {
                display: 'block'
              },
              ref: 'popup',
              on: {
                mouseleave: this.hideOnLeave
              }
            },
            [
              h('div', {'class': 'arrow'}),
              h('h3', {
                'class': 'popover-title',
                directives: [
                  {name: 'show', value: this.title}
                ]
              }, this.title),
              h('div', {
                'class': 'popover-content',
                domProps
              }, [content || this.$slots.popover])
            ]
          )
        ]
      )
    },
    props: {
      title: {
        type: String,
        default: ''
      },
      content: {
        type: String,
        default: ''
      },
      trigger: {
        type: String,
        default: TRIGGERS.OUTSIDE_CLICK
      },
      html: {
        type: Boolean,
        default: false
      }
    },
    computed: {
      allContent () {
        return this.title + this.content
      }
    },
    methods: {
      isNotEmpty () {
        return this.title || this.content || this.$slots.popover
      }
    }
  }
</script>
