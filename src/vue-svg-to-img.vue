<template>
  <div v-if="staticSvg" class="vue-svg-to-png__wrapper">
    <img
      v-if="src && !loading"
      :src="newSrc"
      :alt="alt"
      :width="width"
      :height="height"
      :class="customClass"
      class="vue-svg-to-png"
    />
    <img
      v-else-if="!loading"
      :alt="'No Source Image'"
      :class="customClass"
      class="vue-svg-to-png"
    />
  </div>
  <div v-else :class="customClass" class="vue-svg-to-png__dynamic">
    <div class="vue-svg-to-png__icon" />
  </div>
</template>

<script lang="ts">
import { defineComponent, onMounted, nextTick, ref } from 'vue'

export default defineComponent({
  name: 'VueSvgToImg',
  props: {
    src: {
      type: String,
      default: null
    },
    width: {
      type: [String, Number],
      default: null
    },
    height: {
      type: [String, Number],
      default: null
    },
    alt: {
      type: String,
      default: (value: string) => {
        if (value) return 'No Alt Image'
        return value
      }
    },
    color: {
      type: String,
      default: null
    },
    colorHover: {
      type: String,
      default: null
    },
    static: {
      type: Boolean,
      default: true
    },
    customClass: {
      type: String,
      default: null
    }
  },
  setup(props) {
    const { src, width, height, alt, color, colorHover, static: staticSvg, customClass } = props
    const newSvg = ref<string | null>(null)
    const newSrc = ref<string | null>(null)
    const loading = ref(true)

    const renderingStatic = async () => {
      loading.value = true
      await nextTick(() => {
        const elements = document.querySelectorAll('.vue-svg-to-png')
        elements.forEach((element, key) => {
          element.setAttribute('id', `${element.className}__${key + 1}`)
        })
      })

      await fetch(src).then(async(res) => {
        await res.text().then((result) => {
          if (result.includes('currentColor') && color && color !== '') {
            newSvg.value = result.replaceAll('currentColor', color)
          } else {
            newSvg.value = result
          }
        })
      })
      const svgString: any = newSvg.value
      const decoded = unescape(encodeURIComponent(svgString))
      const srcBase64 = btoa(decoded)
      newSrc.value = `data:image/svg+xml;base64,${srcBase64}`
      loading.value = false
    }

    const renderingDynamic = async() => {
      loading.value = true
        await nextTick(() => {
          const elements = document.querySelectorAll('.vue-svg-to-png__dynamic')
          elements.forEach((element: any, key: number) => {
            element.setAttribute('id', `vue-svg-to-png__dynamic__${key + 1}`)
            if (Number(width) && Number(height)) {
              element.style.width = `${width}px`
              element.style.height = `${height}px`
            } else {
              element.style.width = width.toString()
              element.style.height = height.toString()
            }
            element.children[0].style.maskImage = `url(${src})`
            element.children[0].style.webkitMaskImage = `url(${src})`
            element.children[0].style.backgroundColor = color
            element.addEventListener('mouseenter', changeColor)
            element.addEventListener('mouseleave', changeColor)
          })
        })
        loading.value = false
    }

    const changeColor = (e: any) => {
      if (e.type === 'mouseenter') {
        e.target.children[0].style.backgroundColor = colorHover
      } else {
        e.target.children[0].style.backgroundColor = color
      }
    }

    onMounted(async () => {
      if (staticSvg) {
        await renderingStatic()
      } else {
        await renderingDynamic()
      }
    })

    return {
      src,
      newSrc,
      width,
      height,
      alt,
      staticSvg,
      loading,
      customClass
    }
  }
})
</script>

<style scoped lang="css">
.vue-svg-to-png__wrapper {
  display: inline-block;
}
.vue-svg-to-png__dynamic {
  display: inline-block;
  transition: all .3s ease-in-out;
}
.vue-svg-to-png__dynamic .vue-svg-to-png__icon {
  -webkit-mask-position: center;
	-webkit-mask-repeat: no-repeat;
	-webkit-mask-size: contain;
	-webkit-mask-image: none;
	mask-position: center;
	mask-repeat: no-repeat;
	mask-size: contain;
	mask-image: none;
  width: 100%;
  height: 100%;
}
</style>