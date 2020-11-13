# JS

### 给DOM对象添加监听事件

```vue
// 监听滚动条事件 此示例为vue示例
<script>
    export default {
        mounted() {
            this.$refs.list.addEventListener('scroll', this.handleScroll)
        },
        methods: {
            handleScroll (arg) {
              // this.$refs.list.scrollTop = this.$refs.list.scrollHeight
            }
        }
    }
</script>


```

