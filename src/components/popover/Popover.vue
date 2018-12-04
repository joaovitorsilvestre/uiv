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
    render (h) {
      const domProps = this.html ? {innerHTML: this.content} : {}

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
              }, [this.content || this.$slots.popover])
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
