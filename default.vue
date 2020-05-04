<template>
    <div>
        <!--<vue-headful-->
        <!--    :title="title"-->
        <!--    description="Static description"-->
        <!--/>-->
        <h1 class="accessibility">{{ property.name }}</h1>
        <router-view></router-view>
    </div>
</template>

<script>
    // define(["Vue", "vuex",  "vue-headful"], function(Vue, Vuex,  vueHeadful) {
    define(["Vue", "vuex"], function(Vue, Vuex) {
        return Vue.component("default-component", {
            template: template, // the variable template will be injected
            head: {
                // To use "this" in the component, it is necessary to return the object through a function
                title: function () {
                  return {
                    inner: this.meta.meta_title,
                    separator: ' ', // Leave empty separator
                    complement: ' ' // Leave empty complement
                  }
                },
                meta: function () {
                  return [
                     { name: 'description', id: 'description', content: this.meta.meta_description },
                     { name: 'keywords',  id: 'keywords', content: this.meta.meta_keywords },
                     { property: 'og:title', id: 'og:title', content: this.meta.meta_title },
                     { property: 'og:description', id: 'og:description', content: this.meta.meta_description }
                  ]
                }
            },
            data: function() {
                return {
                    meta: {
                      meta_title: "",
                      meta_description: "",
                      meta_keywords: "",
                      meta_image: ""
                    }
                    
                    // headful
                    // title: 'Dynamic title',
                }
            },
            // headful
            // mounted() {
            //     // dummy async operation to show watcher on properties
            //     setTimeout(() => {
            //         this.title = 'Dynamic async title';
            //     }, 3000);
            // },
            
            beforeRouteEnter (to, from, next) {
                next(vm => {
                    // access to component instance via `vm`
                    vm.meta = vm.findMetaDataByPath(to.path);
                    vm.$emit('updateHead')
                });
            },
            beforeRouteUpdate (to, from, next) {
                this.meta = this.findMetaDataByPath(to.path);
                this.$emit('updateHead')
                next();
            },
            computed: {
                 ...Vuex.mapGetters([
                    'property'
                ]),
                findMetaDataByPath () {
                    return this.$store.getters.findMetaDataByPath;
                }
            },
            metaInfo () {
              return {
                  title: this.meta.meta_title,
                  meta: [
                     { name: 'description', vmid: 'description', content: this.meta.meta_description },
                     { name: 'keywords',  vmid: 'keywords', content: this.meta.meta_keywords },
                     { property: 'og:title', vmid: 'og:title', content: this.meta.meta_title },
                     { property: 'og:description', vmid: 'og:description', content: this.meta.meta_description },
                     { property: 'og:image', vmid: 'og:image', content: this.meta.meta_image }
                  ]
              }
            }
        });
    });
</script>