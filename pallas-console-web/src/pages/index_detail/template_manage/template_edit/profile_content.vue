<template>
    <el-scrollbar>
    <div class="profile-template">
        <el-collapse>
            <el-collapse-item v-for="item in profileData.profile.shards" :key="item.id" :name="item.id">
                <template slot="title">
                    <span style="font-weight: bold;font-size: 14px;">{{item.id.replace(/(\[.*\])(\[.*\])(\[.*\])/g, "$1$3")}}</span>
                    <span style="margin-right: 10px;" class="pull-right">{{item.totalTime}}ms</span>
                </template>
                <div class="profile-tree">
                    <div class="profile-tree-th">
                        <span style="margin-left: 20px;">Type and description</span>
                        <span style="margin-right: 20px;" class="pull-right">Time</span>
                    </div>
                    <el-tree
                        :data="item.searches[0].query"
                        :props="defaultProps"
                        :render-content="renderContent">
                    </el-tree>
                </div>
            </el-collapse-item>
        </el-collapse>
    </div>
    </el-scrollbar>
</template>

<script>
export default {
  props: ['profileData'],
  data() {
    return {
      defaultProps: {
        children: 'children',
        label: 'type',
      },
    };
  },
  methods: {
    renderContent(h, { data }) {
      return h(
        'span',
        [
          h('span', { class: { 'profile-tree-type': true } }, data.type),
          h('span', { class: { 'profile-tree-desc': true }, attrs: { title: data.description } }, data.description.length > 30 ? `${data.description.substring(0, 30)}...` : data.description),
          h('span', { class: { 'pull-right': true, 'profile-tree-time': true } }, `${Number(data.time.replace(/([0-9]+\.[0-9]*)ms/, '$1')).toFixed(3)}ms`),
        ],
      );
    },
  },
};
</script>
<style type="text/css">
.profile-template .el-collapse-item__header {
    padding-left: 5px;
    background-color: #272822;
}
.profile-template .el-collapse-item__content {
    padding: 0px 1px;
}
.profile-tree .profile-tree-th {
    color: #aaa;
    font-size: xx-small;
    background-color: #272822;
}
.profile-tree .el-tree {
    border: none;
    color: #eee;
    background-color: #272822;
}
.profile-tree-type {
    font-size: 14px;
    margin-right: 10px;
}
.profile-tree-desc {
    font-size: 10px;
    color: #bbb;
    font-style: italic;
}
.profile-tree-time {
    margin-right: 10px;
}
</style>
