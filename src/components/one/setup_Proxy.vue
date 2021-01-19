<template>
  <span>{{count}}</span>
  <button @click="add(count)">+</button>
</template>

<script lang="ts">
import {ref} from "vue"
import add from "./utils"
/**定义一个context接口限定context的type */
interface Context {
  attrs?:any
  slots?:any
  emit?:any
}
export default {
  setup(props:any,context:Context){
    const count = ref(0)
    const add:()=>void = function():void{
      count.value++
      proxy()
    }
    /**
     * Vue3的响应式 · 核心
     * 通过Proxy(代理): 拦截对data任意属性的任意(13种)操作, 包括属性值的读写, 属性的添加, 属性的删除等...
     * 通过 Reflect(反射): 动态对被代理对象的相应属性进行特定的操作
     * Proxy：https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Proxy
     * Reflect：https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Reflect
     * 代理Proxy*/
    const proxy:()=>void = function():void{
      /*handle：遍历对象的key，若发现给出了一个不存在与原对象的key，则将这个key赋值为 This key is undefine! */
      let handle:object = {
        get(obj:any,prop:any){
          let isMember:boolean = prop in obj
          if (!isMember) return "This key is undefine!"
          return obj[prop]
        }
      }
      let obj:any =  new Proxy({},handle)
      obj.name = "张三"
      obj.age = 18
      console.log(obj.sex);
    }
    
    /** setup生命周期返回的对象，可直接用于渲染模板 */
    return {
      count,
      add
    }
  }
  
}

/**
 * setup执行的时机：
 *    在beforeCreate之前执行(一次), 此时组件对象还没有创建
 *    this是undefined, 不能通过this来访问data/computed/methods / props
 *    其实所有的composition API相关回调函数中也都不可以
 * 
 *
 */
</script>
