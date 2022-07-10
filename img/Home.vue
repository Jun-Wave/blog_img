<template>
  <a-layout
    id="components-layout-demo-custom-trigger"
    :style="{ height: '100vh' }"
  >
    <a-layout-sider
      v-model="collapsed"
      :trigger="null"
      collapsible
      breakpoint="lg"
      collapsed-width="0"
      @collapse="onCollapse"
      @breakpoint="onBreakpoint"
      :style="{
        height: '100vh',
        position: 'relative',
        width: collapsed ? '0' : slidBoxWidth + 'px',
        flexBasis: collapsed ? '0' : slidBoxWidth + 'px',
        minWidth: collapsed ? '0' : '200px',
        maxWidth: collapsed ? '0' : '400px',
        background: '#fff',
      }"
    >
      <div class="logo" />
      <!-- <div id="drag-btn" ></div> -->
      <a-menu :default-selected-keys="['1']" mode="inline">
        <a-menu-item key="1">
          <a-icon type="pie-chart" />
          <span>首页</span>
        </a-menu-item>
        <a-menu-item key="2">
          <a-icon type="desktop" />
          <span>搜索框</span>
        </a-menu-item>
        <a-sub-menu key="sub1">
          <span slot="title"><a-icon type="user" /><span>好友</span></span>
          <a-menu-item key="3"> Tom </a-menu-item>
          <a-menu-item key="4"> Bill </a-menu-item>
          <a-menu-item key="5"> Alex </a-menu-item>
        </a-sub-menu>
        <!-- 1级导航 -->
        <a-sub-menu key="sub2">
          <!-- 1级导航标题 -->
          <span slot="title"
            ><a-icon type="appstore" /><span>导航页</span></span
          >
          <a-tree
            class="draggable-tree"
            :style="{ marginLeft: '30px' }"
            :default-expanded-keys="expandedKeys"
            draggable
            :tree-data="gData"
          />
        </a-sub-menu>

        <a-menu-item key="20">
          <a-icon type="file" />
          <span>回收站</span>
        </a-menu-item>
      </a-menu>
      <div
        id="drag-btn"
        :style="{
          position: 'absolute',
          right: '-6px',
          top: '0',
          height: '100%',
          width: '6px',
          background: 'transparent',
          cursor: 'w-resize',
        }"
      ></div>
    </a-layout-sider>
    <a-layout>
      <a-layout-header style="background: #fff; padding: 0">
        <a-icon
          class="trigger"
          :type="collapsed ? 'menu-unfold' : 'menu-fold'"
          @click="() => (collapsed = !collapsed)"
        />
      </a-layout-header>
      <a-breadcrumb style="margin: 8px 0">
        <a-breadcrumb-item>Home</a-breadcrumb-item>
        <a-breadcrumb-item>List</a-breadcrumb-item>
        <a-breadcrumb-item>App</a-breadcrumb-item>
      </a-breadcrumb>
      <a-layout-content
        :style="{
          margin: '24px 16px',
          padding: '24px',
          background: '#fff',
          minHeight: '280px',
        }"
      >
        Content
      </a-layout-content>
    </a-layout>
  </a-layout>
</template>

<script>

export default {
  data() {
    return {
      collapsed: false,
      slidBoxWidth: 200,
      gData: [{
    "title": "网页",
    "key": "0-0",
    "children": [
      {
        "title": "前端",
        "key": "0-0-0",
        "children": [
          {
            "title": "js",
            "key": "0-0-0-0"
          },
          {
            "title": "css",
            "key": "0-0-0-1"
          },
          {
            "title": "html",
            "key": "0-0-0-2"
          }
        ]
      }
    ]
  }
],
      expandedKeys: ["0-0"],
    };
  },
  mounted() {
    //绑定侧栏拉伸事件
    this.dragSlidBar(20);
  },
  methods: {
    onCollapse(collapsed, type) {
      console.log(collapsed, type);
    },
    onBreakpoint(broken) {
      console.log(broken);
    },
    /**
     * 拖拽事件、拖拽过程中添加防抖
     * delay: 防抖时间
     */
    dragSlidBar(delay) {
      const _this = this;
      const dragBtn = document.getElementById("drag-btn");
      //鼠标按下
      dragBtn.onmousedown = function (e) {
        //记录鼠标开始位置
        let startX = e.clientX;
        // 修改
        dragBtn.left = dragBtn.offsetLeft;
        //防抖初始时间
        let init_time = 0;
        //给得是document绑定事件
        document.onmousemove = function (e) {
          // 防抖
          let now_time = +new Date();
          if (now_time - init_time > delay) {
            init_time = now_time;
            //记录鼠标位置
            let endX = e.clientX;
            //计算鼠标移动距离
            let moveX = endX - startX;
            // 更新鼠标初始位置

            startX = endX;
            //更新盒子宽度
            _this.slidBoxWidth += moveX;
            // 边界值处理
            _this.slidBoxWidth > 400 && (_this.slidBoxWidth = 400);
            _this.slidBoxWidth < 200 && (_this.slidBoxWidth = 200);
            
          }
          return false;
        };
        //给得是document绑定事件
        document.onmouseup = function () {
          document.onmousemove = null;
          document.onmouseup = null;
          // 释放鼠标
          dragBtn.releaseCapture && dragBtn.releaseCapture();
        };
        // 捕获鼠标
        dragBtn.setCapture && dragBtn.setCapture();
        return false;
      };
    },
  },
};
</>
<style>
#components-layout-demo-custom-trigger .trigger {
  font-size: 18px;
  line-height: 64px;
  padding: 0 24px;
  cursor: pointer;
  transition: color 0.3s;
}

#components-layout-demo-custom-trigger .trigger:hover {
  color: #1890ff;
}

#components-layout-demo-custom-trigger .logo {
  height: 32px;
  background: rgba(255, 255, 255, 0.2);
  margin: 16px;
}
</style>
