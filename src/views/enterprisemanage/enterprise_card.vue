<template>
  <div>
    <!-- 搜索条件和添加企业信息 -->
    <div class="searchH">
      <!-- pc搜索条件-->
     <div class="searchT">
       <span class="searchText" >企业名称：</span>
       <el-input
         placeholder="请输入内容"  v-model="pcsearchenterPriseInfo.name" clearable style="width: 10%;" >
       </el-input>
       <span  class="searchText" style="margin-left: 1.25rem;margin-right: 5px;">企业类型：</span>
       <el-select v-model="pcsearchenterPriseInfo.type" placeholder="请选择"  style="width: 10%;">
           <el-option
             v-for="item in options"
             :key="item.value"
             :label="item.label"
             :value="item.value">
           </el-option>
         </el-select>
         <span  class="searchText" style="margin-left: 1.25rem;margin-right: 5px;">创建时间：</span>
           <el-date-picker
               style="width: 20%;"
                v-model="pcsearchenterPriseInfo.time"
                type="datetimerange"
                start-placeholder="开始日期"
                end-placeholder="结束日期"
                :default-time="['12:00:00']">
              </el-date-picker>
              <span  class="searchText" style="margin-left: 1.25rem;margin-right: 5px;">按设备数排序：</span>
              <el-select v-model="pcsearchenterPriseInfo.sort" placeholder="请选择" style="width: 100px;">
                  <el-option
                    v-for="item in optionsN"
                    :key="item.value"
                    :label="item.label"
                    :value="item.value">
                  </el-option>
                </el-select>

                <span  class="searchText" style="margin-left: 1.25rem;margin-right: 5px;">是否启用：</span>
                <el-select v-model="pcsearchenterPriseInfo.isUser" placeholder="请选择" style="width: 100px;">
                    <el-option
                      v-for="item in optionsIS"
                      :key="item.value"
                      :label="item.label"
                      :value="item.value">
                    </el-option>
                  </el-select>

              </div>

    <!-- 搜索、添加企业 -->
     <div style="display: flex;margin-right: 160px;">
          <el-button type="primary" icon="el-icon-search" round
           @click.native="searchDo"   size='mini'>搜索</el-button>
          <el-button type="primary" round   @click="adddialogVisible=true" size='mini'>添加企业</el-button>
     </div>
     </div>
    <!-- 公司信息card -->
     <div  class="enterpriseCard">
     <el-card class="box-card" v-for=" i in 12 " :key='i'>
        <div style="display: flex;justify-content: space-between;">
           <el-button round size="small"
           style="background-color: #55C2DD;color: #FFFFFF;" >正式企业&nbsp;|&nbsp;启用</el-button>
           <div class="div_number_bg">{{i}}</div>
        </div>
         <div class="card_text">山东领先集团</div>
         <div class="card_text"><span>负责人：</span><span>周一</span></div>
         <div class="card_text"><span>电&nbsp;话:</span><span>13462268043</span></div>
         <div class="card_text">地址：山东省东营市高新技术产业聚集区</div>
         <div class="card_text">创建时间：2020-11-5</div>
         <div style="background-color: #F0F2F5;height: 1px;margin: 3px 3px 3px 3px;" ></div>
         <div class="card_text">总设备：522台</div>
         <div class="card_text"><span>操作：</span>
         <!-- <el-button type="success" plain size="mini" round>查看</el-button> -->
         <el-button type="warning" plain size="mini" round @click="updatedialogVisible = true">修改</el-button>
         <el-button type="danger" plain size="mini" round @click="delDialogVisible = true">删除</el-button>
       </div>
     </el-card>
    </div>
     <!-- 分页 -->
    <el-pagination
      style="width: 100%; text-align: right"
      background
      hide-on-single-page
      layout="prev, pager, next"
      :total="100">
    </el-pagination>

    <!-- 修改信息对话框 -->
    <el-dialog
      title="修改企业信息"
       width="35%"
      :visible.sync="updatedialogVisible"

      style="text-align: center;"
      >
     <div style="margin-left: 1.875rem;">
        <div style="display: flex;align-items: center;">
           <span>企业名称：</span>
           <el-input
             v-model="updatedialogVisible.name" clearable style="width: 200px;">
           </el-input>
        </div>
        <div style="display: flex;align-items: center;margin-top: 10px;">
          <span>企业类型：</span>
          <el-select v-model="updateenterPriseInfo.type" placeholder="请选择" style="width: 200px;">
              <el-option
                v-for="item in options"
                :key="item.value"
                :label="item.label"
                :value="item.value">
              </el-option>
            </el-select>
        </div>
        <div style="display: flex;align-items: center;margin-top: 10px;">
           <span>公司地址：</span>
           <el-input
             v-model="updateenterPriseInfo.address" clearable style="width: 200px;">
           </el-input>
        </div>
        <div style="display: flex;align-items: center;margin-top: 10px;">
           <span>联   系  人：</span>
          <el-input
            v-model="updateenterPriseInfo.contactPerper" clearable style="width: 200px;">
          </el-input>
        </div>
        <div style="display: flex;align-items: center;margin-top: 10px;">
           <span>联系电话：</span>
          <el-input
            v-model="updateenterPriseInfo.phone" clearable style="width: 200px;">
          </el-input>
        </div>
        <div style="display: flex;align-items: center;margin-top: 10px;margin-bottom: 20px;">
           <span>是否启用：</span>
         <el-select v-model="updateenterPriseInfo.isUser" placeholder="请选择" style="width: 100px;">
             <el-option
               v-for="item in optionsIS"
               :key="item.value"
               :label="item.label"
               :value="item.value">
             </el-option>
           </el-select>
        </div>
        <span slot="footer" class="dialog-footer">
          <el-button @click="updatedialogVisible = false">取 消</el-button>
          <el-button type="primary" @click="updatedialogVisible = false">修改保存</el-button>
        </span>
      </div>
    </el-dialog>
    <!-- 删除对话框 -->
      <el-dialog
        title="提示"
        :visible.sync="delDialogVisible"
        width="25%"
        center

        >
        <span >确定删除山东领先集团公司信息</span>
        <span slot="footer" class="dialog-footer">
          <el-button @click="delDialogVisible = false">取 消</el-button>
          <el-button type="primary" @click="delDialogVisible = false">确 定</el-button>
        </span>
      </el-dialog>

      <!-- 平板搜索条件-->
    <el-dialog title="搜索条件" :visible.sync="searchdialogVisible" width="48%" style="text-align: center;">
      <el-form :model="minisearchenterPriseInfo">
         <div style="margin: 0.625rem;">
          <span class="searc" >企业名称：</span>
          <el-input
            placeholder="请输入企业名称"  v-model="minisearchenterPriseInfo.name" clearable style="width: 48%;" >
          </el-input>
          </div>
          <div style="margin: 0.625rem;">
              <span  class="searchText" style="">企业类型：</span>
              <el-select v-model="minisearchenterPriseInfo.type" placeholder="请选择"  style="width: 48%;">
                  <el-option
                    v-for="item in options"
                    :key="item.value"
                    :label="item.label"
                    :value="item.value">
                  </el-option>
                </el-select>
            </div>
            <div style="margin: 0.625rem;">
              <span  class="searchText" style="">创建时间：</span>
                <el-date-picker
                    style="width: 48%;"
                     v-model="minisearchenterPriseInfo.time"
                     type="datetimerange"
                     start-placeholder="开始日期"
                     end-placeholder="结束日期"
                     :default-time="['12:00:00']">
                   </el-date-picker>
            </div>
           <div style="margin: 0.625rem;">
                <span   style="">是否启用：</span>
                <el-select v-model="minisearchenterPriseInfo.isUser" placeholder="请选择"
                style="width: 48%;">
                    <el-option
                      v-for="item in optionsIS"
                      :key="item.value"
                      :label="item.label"
                      :value="item.value">
                    </el-option>
                  </el-select>
            </div>
            <div style="margin: 0.625rem;">
                 <span  class="searchText" style="">按设备数排序：</span>
                 <el-select v-model="minisearchenterPriseInfo.sort" placeholder="请选择" style="width: 42%;">
                     <el-option
                       v-for="item in optionsN"
                       :key="item.value"
                       :label="item.label"
                       :value="item.value">
                     </el-option>
                   </el-select>
             </div>
         </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="searchdialogVisible = false">取 消</el-button>
        <el-button type="primary" @click="searchdialogVisible = false">确 定</el-button>
      </div>
    </el-dialog>

    <!--添加企业信息对话框-->
    <el-dialog
      title="添加企业信息"
       width="35%"
      :visible.sync="adddialogVisible"

      style="text-align: center;"
      >
     <div style="margin-left: 1.875rem;">
        <div style="display: flex;align-items: center;">
           <span>企业名称：</span>
           <el-input
             v-model="addenterPriseInfo.name" clearable style="width: 200px;">
           </el-input>
        </div>
        <div style="display: flex;align-items: center;margin-top: 10px;">
          <span>企业类型：</span>
          <el-select v-model="addenterPriseInfo.type" placeholder="请选择" style="width: 200px;">
              <el-option
                v-for="item in options"
                :key="item.value"
                :label="item.label"
                :value="item.value">
              </el-option>
            </el-select>
        </div>
        <div style="display: flex;align-items: center;margin-top: 10px;">
           <span>公司地址：</span>
           <el-input
             v-model="addenterPriseInfo.address" clearable style="width: 200px;">
           </el-input>
        </div>
        <div style="display: flex;align-items: center;margin-top: 10px;">
           <span>联   系  人：</span>
          <el-input
            v-model="addenterPriseInfo.contactPerper" clearable style="width: 200px;">
          </el-input>
        </div>
        <div style="display: flex;align-items: center;margin-top: 10px;">
           <span>联系电话：</span>
          <el-input
            v-model="addenterPriseInfo.phone" clearable style="width: 200px;">
          </el-input>
        </div>
        <div style="display: flex;align-items: center;margin-top: 10px;">
           <span>创建时间：</span>
           <el-date-picker
           style="width: 200px;"
                v-model="addenterPriseInfo.creattime"
                type="datetime"
                placeholder="选择日期时间"
                default-time="12:00:00">
              </el-date-picker>
        </div>
        <div style="display: flex;align-items: center;margin-top: 10px;margin-bottom: 20px;">
           <span>是否启用：</span>
         <el-select v-model="addenterPriseInfo.isUser" placeholder="请选择" style="width: 100px;">
             <el-option
               v-for="item in optionsIS"
               :key="item.value"
               :label="item.label"
               :value="item.value">
             </el-option>
           </el-select>
        </div>
        <span slot="footer" class="dialog-footer">
          <el-button @click="adddialogVisible = false">取 消</el-button>
          <el-button type="primary" @click="adddialogVisible = false">修改保存</el-button>
        </span>
      </div>
    </el-dialog>


  </div>
</template>

<script>
export default {
    data() {
      return {
        options: [{
          value: '正式企业1',
          label: '正式企业'
        }, {
          value: '试用企业2',
          label: '试用企业'
        },],
        optionsIS: [{
          value: '是',
          label: '是'
        }, {
          value: '否',
          label: '否'
        },],
        optionsN: [{
          value: '顺序',
          label: '顺序'
        }, {
          value: '倒序',
          label: '倒序'
        },],
         screenWidth:'',
         searchdialogVisible:false,
         adddialogVisible:false,
         updatedialogVisible: false,
         delDialogVisible: false,
         addenterPriseInfo:{
           name:'',
           type:'',
           address:'',
           contactPerper:'',
           phone:'',
           isUser:'',
           creattime:'',
         } ,
         updateenterPriseInfo:{
           name:'',
           type:'',
           address:'',
           contactPerper:'',
           phone:'',
           isUser:'',
           creattime:'',
         },
         pcsearchenterPriseInfo:{
           name:'',
           type:'',
           address:'',
           contactPerper:'',
           phone:'',
           isUser:'',
           time:'',
           sort:'',
         },
         minisearchenterPriseInfo:{
           name:'',
           type:'',
           address:'',
           contactPerper:'',
           phone:'',
           isUser:'',
           time:'',
           sort:'',
         },


      }
    },
    methods: {
          /*
          搜索按钮
          */
          searchDo(){
           this.screenWidth= document.body.clientWidth;
           if(this.screenWidth>1100){
             /* 表示pc端，弹窗隐藏*/
              this.searchdialogVisible=false,
              console.log(this.pcsearchenterPriseInfo)

           }else{
             /* 表示平板端,弹窗显示*/
             this.searchdialogVisible=true,
              console.log(this.minisearchenterPriseInfo)
           }

          },
        },

  }
</script>

<style scoped="scoped">
  *{
   font-family: Microsoft Yahei;
    padding: 0rem;
    margin: 0rem;
  }
    .searchH{
      width: 100%;
      display: flex;

      color: gray;
      margin-top: -10px;
      margin-left: 70px;
    }
    .searchText{
      margin-left: 0.2rem;
     margin-right: 0.1rem;
    }
   .update-dialog-1{
     width: 20%;
   }
   .qiyecreattime{
       width:100px;
     }
    .box-card{
      width: 18%;
      color: gray;
      margin: 8px;

    }
     .el-card__body{
       padding: 10px;
     }
    .card_text{
      padding: 5px 0px 5px 12px ;


    }
    .div_number_bg{
      background-image: url(../../assets/img/number_bg.png);
      background-repeat: no-repeat;
      background-size: auto;
      width: 15px;
      height: 25px;
      text-align: center;
      font-size: 12px;
      color: #FFFFFF;
    }
    .enterpriseCard{
       margin:10px 20px 20px 60px;
       display: flex;
       flex-wrap: wrap;
    }

@media screen and (max-width: 1024px) {

  *{
    font-size: 14px;
    font-family: Microsoft Yahei;
    padding: 0;
    margin: 0;

  }
  /* 隐藏pc的搜索条件*/
  .searchT{
    display: none;
  }
 .searchH{
   width: 100%;
   display: flex;
   justify-content: space-between;
   color: gray;

   margin-left: 30px;

 }


   .enterpriseCard{
      margin:5px 20px 20px 20px;
      display: flex;
      flex-wrap: wrap;
   }
  .enterpriseInput{
      width: 21%;
      padding: 0rem;
      margin: 0rem;
    }
   .qiyetypeselect{
      width: 20%;
    }
    .qiyecreattime{
       width: 5%;
     }
    .box-card {
      width: 30%;
      color: gray;
      margin: 8px;

    }
     .el-card__body{
       padding: 10px;
     }
    .card_text{
      padding: 5px 0px 5px 12px ;


    }
    .div_number_bg{
      background-image: url(../../assets/img/number_bg.png);
      background-repeat: no-repeat;
      background-size: auto;
      width: 15px;
      height: 25px;
      text-align: center;
      font-size: 12px;
      color: #FFFFFF;
    }
}

</style>
