<template>
  <div>
    <el-dialog title="菜单添加" :visible.sync="info.isShow">
      <el-form :model="form">
        <el-form-item label="菜单名称" :label-width="width">
          <el-input v-model="form.title" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="上级菜单" :label-width="width">
          <el-select v-model="form.pid" placeholder="请选择活动区域">
            <el-option label="顶级菜单" value="0"></el-option>
          </el-select>
        </el-form-item>

        <el-form-item label="菜单类型" :label-width="width">
          <el-radio v-model="form.type" :label="1">目录</el-radio>
          <el-radio v-model="form.type" :label="2">菜单</el-radio>
        </el-form-item>
        <el-form-item label="菜单图标" :label-width="width" v-if="form.type==1">
          <el-select v-model="form.icon" placeholder="请选择活动区域">
            <el-option label="星星" value="el-icon-star-on">
              <i class="el-icon-star-on"></i>
            </el-option>
            <el-option label="时钟" value="el-icon-success">
              <i class="el-icon-success"></i>
            </el-option>
            <el-option label="信息" value="el-icon-s-comment">
              <i class="el-icon-s-comment"></i>
            </el-option>
            <el-option label="图片" value="el-icon-picture">
              <i class="el-icon-picture"></i>
            </el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="菜单地址" :label-width="width" v-else>
          <el-select v-model="form.url" placeholder="请选择活动区域">
            <el-option
              v-for="item in indexRouters" :key="item.path" :value="item.name" :label="'/'+item.path">           
          </el-option>
          </el-select>
        </el-form-item>
        <el-form-item lable="状态" :label-width="width">
          <el-switch v-model="form.status" active-color="#13ce66" inactive-color="#ff4949" :active-value="1" :inactive-value="2">         
          </el-switch>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="info.isShow = false">取消</el-button>
        <el-button type="primary" @click="add" v-if="info.isAdd">添加</el-button>
        <el-button type="primary" @click="updata(form)" v-else>修改</el-button>
      </div>
    </el-dialog>
  </div>
</template> 

<script>
import {indexRouters} from "../../../router/index";
import {reqMenuAdd,reqMenuOne,reqMenuEdit} from "../../../util/request";
import {mapActions,mapGetters} from "vuex";
export default {
  props: ['info'],
  data() {
    return {
      //  isShow:true,
      form: {
        pid: 0,
        title: "",
        icon: "",
        type: 1,
        url: "",
        status: 1,
      },
      width: "100px",
      indexRouters: indexRouters,
    };
  },
  computed: {},
  watch: {},
  methods: {
    //让弹框隐藏
    hide() {
      this.info.isShow = false;
    },
    //重置数据，清空之前的输入
    empty(){
       this.form.pid=0,
           this.form.title='',
           this.form.icon='',
           this.form.type=1,
           this.form.url='',
           this.form.status=1
    }, 
    //添加数据到数据库
    add() {
      reqMenuAdd(this.form).then(res => {
        this.hide();
        this.reqMenuList();
        
      }); 
    },
    ...mapActions({
      reqMenuList:'menu/reqMenuList'
    }),
    // 获取一条数据
    look(id){
      reqMenuOne({id:id}).then(res=>{
          this.form = res.data.list
          // 目的就是为了更新数据的时候获取id
          this.form.id=id
      })
    },
    updata(form){
      reqMenuEdit(this.form).then(res=>{
            this.hide()
            this.reqMenuList() 
            //重置数据
            this.empty()
      })
    }
  },
  mounted(){
    
  }
};
</script>

<style lang='' scoped>
</style>
