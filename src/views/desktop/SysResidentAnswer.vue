<template> 
 <div class="block">   
	 	<el-col :span="20" class="toolbar" style="padding-bottom: 0px;">
			<el-form :inline="true">
				<el-form-item style="width:300px">
					<el-input v-model="remark"  @keyup.enter.native="query"  placeholder="请输入姓名/手机号/住院号" style="width:300px"></el-input>
				</el-form-item>
            


				<el-form-item  >
					<el-button type="primary" v-on:click="query">查询</el-button>
				</el-form-item>
                <!-- <el-form-item  >
					<el-button type="success" v-on:click="query">刷新</el-button>
				</el-form-item> -->
				
				
			</el-form>
		</el-col>
        <el-col :span="4" class="toolbar" style="padding-bottom: 0px;">
			<el-form :inline="true">
				
				<el-form-item  style="text-align:right">
					<el-button type="primary" @click="add()">添加患者信息</el-button>
				</el-form-item>

                <!-- <el-form-item  style="text-align:right">
					<el-button type="primary" @click="updateDevice()">绑定设备</el-button>
				</el-form-item> -->
			</el-form>
		</el-col>
      
        
		<el-table :data="datalist" highlight-current-row v-loading="listLoading" style="width: 100%;">
		
			<!-- <el-table-column prop="type" label="素材类型" width="120" sortable>
                <template slot-scope="scope">{{scope.row.type =='10'?'图片':'视频' }}</template>
			</el-table-column> -->
			 <el-table-column prop="name" label="姓名" width="150">
			</el-table-column>
            <el-table-column prop="phone" label="联系方式" width="160" sortable>
			</el-table-column>
            <el-table-column prop="sex" label="性别" width="100" sortable>
                <template slot-scope="scope">{{scope.row.sex =='10'?'男':'女' }}</template>
			</el-table-column>

            <el-table-column prop="classes" label="问卷类别" width="180" sortable>
                <template slot-scope="scope">{{scope.row.classes =='10'?'术前评估1':scope.row.classes =='20'?'术前评估2':scope.row.classes =='30'?'术后评估1':scope.row.classes =='40'?'术后评估2':scope.row.classes =='50'?'术后评估3':'' }}</template>
			</el-table-column>
         

             <el-table-column prop="type" label="类型" width="120" sortable>
                 <template slot-scope="scope">{{ scope.row.type=='10'?'左脚':'右脚'}}</template>
			</el-table-column>

            <el-table-column  label="生成时间" width="170">
				<template slot-scope="scope">{{ scope.row.createTime | moment('YYYY-MM-DD HH:mm:ss') }}</template>
			</el-table-column>


            <el-table-column  label="状态" width="140">
				<template slot-scope="scope">{{ state(scope.row.state)}}</template>
			</el-table-column>

             <el-table-column prop="loginName" label="主管医生" width="200" sortable>
			</el-table-column>
			 <!-- <el-table-column prop="remark" label="备注" width="200">
				
			</el-table-column>
			 -->
            <el-table-column label="操作" min-width="160">
				<template scope="scope">
				<!-- <el-button size="small" type="primary"  @click="edit(scope.$index,scope.row)">编辑</el-button> -->
                <el-button size="small" type="primary"  @click="edit(scope.$index,scope.row)">查看问卷</el-button>
				 <!--
                <el-button size="small" type="primary"  v-if='scope.row.sysUserId=="" ||  scope.row.sysUserId ==null' @click="addAdmin(scope.$index,scope.row)">新增物业</el-button>
				<el-button size="small" type="warning"  v-if='scope.row.sysUserId!="" ||  scope.row.sysUserId !=null' @click="editAdmin(scope.$index,scope.row)">修改物业</el-button> -->
                <el-button size="small" type="danger" @click="deleteRow(scope.$index, scope.row)">删除</el-button>
                <!-- <el-button size="small" type="warning" @click="updateRow(scope.$index, scope.row)">替换</el-button> -->
				</template>
			</el-table-column>

            <!-- <el-table-column prop="fileUrl" label="文件地址" width="200" sortable>
                <template slot-scope="scope"><img :src="scope.row.fileUrl"></template>
			</el-table-column> -->
            
		
		</el-table>

		<el-pagination
		 	class="pull-right clearfix"
			@current-change="handleCurrentChange"
			:current-page="currentPage"
			:page-size="totalsize" 
			layout="total, prev, pager, next, jumper"
			:total="total" 
		>
		</el-pagination>

        <el-dialog  class="customWidth"  :title="formtitle" :visible.sync="dialogFormVisible" >
			<el-form ref="subData" :model="subData" label-width="100px" @submit.native.prevent  style="margin:20px;"  id="formid" action=""  method="post" enctype="multipart/form-data" target="frameName">
                 
                    
                    <div style="text-align:center;font-size:16px; font-weight:bold">用户基本信息</div>
                    
                 
                      <el-row class="height1">
                            <table id="table1" style="width:100%;">
                                <tr>
                                    <td class="checkTitle">姓名:</td>    
                                    <td class="checkText">{{subData.name}}</td>
                                     <td class="checkTitle"> 性别:</td>    
                                    <td  class="checkText">{{subData.sex=='10'?'男':'女'}}</td>
                                     <td class="checkTitle">年龄:</td>    
                                    <td  class="checkText">{{subData.age}}</td>
                                     <td class="checkTitle">住院号:</td>    
                                    <td  class="checkText">{{subData.cardNo}}</td>        
                                </tr>    


                                <tr>
                                    <td class="checkTitle">民族:</td>    
                                    <td class="checkText">{{subData.nation}}</td>
                                     <td class="checkTitle"> 身高(cm):</td>    
                                    <td  class="checkText">{{subData.height1}}</td>
                                     <td class="checkTitle">体重(kg):</td>    
                                    <td  class="checkText">{{subData.weight1}}</td>
                                     <td class="checkTitle">患病位置:</td>    
                                    <td  class="checkText">{{subData.type=='10'?'左脚':'右脚'}}</td>        
                                </tr>

                                <tr>
                                    <td class="checkTitle">职业:</td>    
                                    <td class="checkText">{{subData.vocation}}</td>
                                     <td class="checkTitle">学历:</td>    
                                    <td  class="checkText">{{subData.record}}</td>
                                     <td class="checkTitle">联系号码:</td>    
                                    <td  class="checkText" colspan="2">{{subData.phone}}</td>
                                     <td class="checkTitle"></td>    
                                   
                                </tr> 

                                <tr>
                                    <td class="checkTitle">地址:</td>    
                                    <td colspan="6" class="checkText">{{subData.address}}</td>
                                </tr>       
                            </table>    
                      </el-row>

                     
                    <div style="text-align:center;font-size:16px; font-weight:bold">(医生完成)</div>
                   
                    <el-row class="height1">

                        <el-col :span="24" :label="m.id" :key="m.id"  v-for=" m in classes1List.answerList" style="margin:10px 0px; font-size:17px;">
                                 
                                 <div style="text-align:center;font-size:16px; font-weight:bold;margin-bottom:10px">{{m.titleType}}</div>
                                  <div style="text-align:center;font-size:16px; font-weight:bold">{{m.titleType1}}</div>
                                 <div style="margin-bottom:10px;font-size:16px; font-weight:bold;">{{m.name}}<span style="color:red;margin-left:50px;font-size:14px"> 得分:{{m.remark}}<span v-if="m.type=='30'?m.message:''">{{Math.ceil((parseInt(m.message.split(",")[1])-parseInt(m.message.split(",")[0]))/5+"")}}</span></span></div>   
                                 <div>
                                       <el-row :span="24"> 
                                           <span style="color:green">{{m.type=='30'?m.message:""}}</span>          
                                           <el-col style="width:25%;"  :label="b.id" :key="b.id"  v-for=" b in m.answerList"   >
                                         
                                         <span v-if="m.message != b.answer" style="color:#000">{{b.answer}}  ({{b.remark}}分)</span> <span v-if="m.message == b.answer" style="color:green">{{b.answer}} ({{b.remark}}分)</span> 
                                           </el-col>    

                                       </el-row>        

                                 </div> 
                         </el-col>

                    
                    </el-row>    

                    <div style="text-align:center;font-size:16px; font-weight:bold">(患者完成)</div>

                    <el-row class="height1">

                         <el-col :span="24" :label="m.id" :key="m.id"  v-for=" m in classes2List.answerList" style="margin:10px 0px; font-size:17px;">
                                 <!-- type=='20'是多选 -->
                                  <div style="text-align:center;font-size:16px; font-weight:bold;margin-bottom:10px">{{m.titleType}}</div>
                                  <div style="text-align:center;font-size:16px; font-weight:bold">{{m.titleType1}}</div>
                                 <div style="margin-bottom:10px;font-size:16px; font-weight:bold;" v-if="m.type!='20'">{{m.name}}<span style="color:red;margin-left:50px;font-size:14px"> 得分:{{m.remark}}</span></div>   
                                 <div>
                                       <el-row :span="24" v-if="m.type=='10' && m.answerList.length<10" > 
                                           <span style="color:green">{{m.type=='30'?m.message:""}}</span>          
                                           <el-col   style="width:33%;"  :label="b.id" :key="b.id"  v-for=" b in m.answerList"   >
                                                  <span v-if="m.message != b.answer" style="color:#000">{{b.answer}}  ({{b.remark}}分)</span> <span v-if="m.message == b.answer" style="color:green">{{b.answer}} ({{b.remark}}分)</span> 
                                           </el-col>    

                                       </el-row>      


                                        <el-row :span="24" v-if="m.type=='10' && m.answerList.length>=10" > 
                                           <span style="color:green">{{m.type=='30'?m.message:""}}</span>          
                                           <el-col   style="width:9%;"  :label="b.id" :key="b.id"  v-for=" b in m.answerList"   >
                                                  <span v-if="m.message != b.answer" style="color:#000">{{b.answer}}  ({{b.remark}}分)</span> <span v-if="m.message == b.answer" style="color:green">{{b.answer}} ({{b.remark}}分)</span> 
                                           </el-col>    

                                       </el-row>        

                                        <el-row :span="24" v-if=" m.answerList.length==0" > 
                                                <!-- {{manyAnswer.length}} -->

                                            <el-col   style="width:33%;"  :label="b.id" :key="b.id"  v-for=" b in manyAnswer"   >
                                                  <span v-if="m.message != b.answer" style="color:#000">{{b.answer}}  ({{b.remark}}分)</span> <span v-if="m.message == b.answer" style="color:green">{{b.answer}} ({{b.remark}}分)</span>
                                           </el-col>

                                       </el-row>        

                                        <br/><br/>   
                                        

                                      

                                 </div> 
                               
                         </el-col>

                    
                    </el-row>    

                      <!-- <div style="font-size:24px;color:red;font-weight:bold">
                                            总得分：{{sunCount}}
                                        </div>     -->

                    
                   

                   
            


			</el-form>	
			<div slot="footer" class="dialog-footer">
				<el-button @click="dialogFormVisible = false">取 消</el-button>
				<el-button type="primary"  @click="open()">确 定</el-button>
			</div>
        </el-dialog>


        <el-dialog   :title="formtitle" :visible.sync="dialogFormVisible2" >
			<el-form ref="subData" :model="subData" label-width="100px" @submit.prevent="onSubmit" style="margin:20px;"  id="formid" action=""  method="post" enctype="multipart/form-data" target="frameName">
                    
                    <!-- <el-form-item label="设备类型">
                       <el-select  v-model="subData.equipmentType" placeholder="请选择设备类型" @change="selectEquType(subData.equipmentType)">
                           <el-option :label="item.label" :key="item.value" :value="item.value" v-for="item in options">{{item.label}}</el-option>
					    </el-select>
                    </el-form-item>  -->

                    <el-form-item label="文件类型">
					<el-radio-group v-model="subData.type">
						<el-radio label="10">图片</el-radio>
						<!-- <el-radio label="20">视频</el-radio> -->
					</el-radio-group>
                     </el-form-item>
                    <!-- <el-form-item label="规格">
					<el-radio-group v-model="subData.standards">
						<el-radio label="10">427*400</el-radio>
						<el-radio label="20">640*400</el-radio>
                        <el-radio label="30">853*400</el-radio>
                        <el-radio label="40">1280*400</el-radio>
					</el-radio-group>
                     </el-form-item>    -->

                    <!-- <el-form-item label="*设备编号">
                        <el-input v-model="subData.equNo" type="number"  placeholder="请输入6位设备编号"></el-input>
                    </el-form-item> -->
                   <el-form-item label="*设备编号"> 
                   <el-upload
                    class="upload-demo"
                    action="http://47.98.164.234:8080/cms/file/upload"

                    multiple="false"
                    
                    :data="subData"
                    limit="1"
                    :on-preview="handlePreview"
                    :on-success="uploadok"
                    :on-remove="handleRemove"
                    :file-list="fileList"
                    list-type="picture">
                    <el-button size="small" type="primary">点击上传</el-button>
                    <div slot="tip" class="el-upload__tip">只能上传图片jpg/png格式,分辨率为600*600</div>
                    </el-upload>        
                    </el-form-item>
                    
                 
                    <el-form-item label="状态">
                        <el-radio-group  name="state" v-model="subData.state">
                            <el-radio label="10">正常</el-radio>
                            <el-radio label="20">禁用</el-radio>
                        </el-radio-group>
                    </el-form-item>
        
                    <el-form-item label="备注">
                        <el-input name="remark"  v-model="subData.remark"  placeholder="请输入备注"></el-input>
                    </el-form-item>


			</el-form>	
			<div slot="footer" class="dialog-footer">
				<el-button @click="dialogFormVisible2 = false">取 消</el-button>
				<el-button type="primary" @click="open()">确 定</el-button>
			</div>
        </el-dialog>



      

  </div>
</template>
<script>
	//2
	import { RequestPost } from '../../api/api';
    import { RequestGet } from '../../api/api';
    import { url } from '../../api/api';
    import { fileUrls } from '../../api/api';
    
	import {timeFormat} from '../../api/format';
	import {dateFormat} from '../../api/format';
	import {state} from '../../api/format';
	import { PageSize } from '../../api/api';
	import moment from 'moment';
  	export default {
	  
    methods: {
	  dateFormat,	
	  timeFormat,
      state,	
      


    handleRemove(file, fileList) {
       // alert(file+":::"+fileList);
       this.fileList = [];
       this.disabledBtn=false;
        console.log(file, fileList);
      },
      handlePreview(file) {
        alert(file);
        console.log(file);
      },
      uploadok(res,file){  //上传成功后调用方法
        this.paths = this.paths+res.data+",";
        this.disabledBtn=true;
         
          
      },
      handleSizeChange(val) {
		console.log(`每页 ${val} 条`);
		

      },
      
      handleCurrentChange(val) {
		console.log(`当前页: ${val}`);
		this.pageNumber = val;

		
		this.page.pageNumber = val;
		/**
		 * 1.get 请求传参数，必须是json前面加一个params
		 * 2.post传参数不需要在json对象传值前面带params
		 */
		this.loadData();


	  },
       /**
        * 查询
        */
        query(){
            this.loadData();
        },

        addByEnterKey(e){
            alert(e);
            if (e.keyCode == 13) {
             this.open();
            }
                



        },
        

      add(){

           this.fileList = [];     
           this.disabledBtn  = false;
           this.dialogFormVisible = true;
		   this.formtitle ="新增素材";   
           this.subData = {}; 
          
            this.isEdit = true;
            this.subData = {
                 type:"10",
                 sex:"10",
                 state:"10",
                 vocation:"",
                 record:"",
                 userId:sessionStorage.getItem("userId")
                
            };

            this.paths = "";
            this.subData.userName = sessionStorage.getItem("loginName");
           this.subData.userId = sessionStorage.getItem("userId"); 


      },
       edit(index, rows){
            this.isEdit = true;
             this.dialogFormVisible = true;
		    this.formtitle ="查看问卷";   
           
            this.residentId = rows.id;
            
            


            

             RequestGet("/sysResident/findOne?id="+rows.sysResidentId).then(response => {
						if(response.code == '0000'){
                            this.subData = response.data;


                                var person1={
                                    residentId:rows.sysResidentId,
                                    type: this.subData.type,
                                    classes:""
                                }

                                RequestGet("/sysResidentAnswer/getAnswer",person1).then(response => {


                                            
                                            if(response.code == '0000'){

                                                for(var i =0;i<response.data.length;i++){
                                                    if(response.data[i].classes=='10'){
                                                        this.classes2List = response.data[i];
                                                        console.log(this.classes2List.answerList);
                                                    for(var j=0;j<this.classes2List.answerList.length;j++){
                                                                
                                                                if(this.classes2List.answerList[j].type=='20'){
                                                                        this.manyAnswer = this.classes2List.answerList[j].answerList;

                                                                }

                                                            this.sunCount+=   parseInt(this.classes2List.answerList[j].remark)  ;
                                                    }    

                                                        
                                                    }
                                                    if(response.data[i].classes=='20'){
                                                        this.classes1List = response.data[i]    
                                                    }  
                                                }    

                                                
                                            }
                                        
                                }).catch(error => {
                                                this.$router.push({ path: '/login' });
                                                
                                }) 
                          
						 }
					
            }).catch(error => {
                            this.$router.push({ path: '/login' });
                            
            }) 
            
            


           

      },
      
       deleteRow(index, rows) {
       this.$confirm('确认删除, 是否继续?', '提示', {
            confirmButtonText: '确定',
            cancelButtonText: '取消',
            type: 'warning'
            }).then(() => {
           

          // rows.userName = this.userName;

            //rows.state = '20';        
            //删除功能暂时不开放
            RequestPost("/sysResident/delete",rows).then(response => {
						
						//this.logining = false; 
                if(response.code=='0000'){
                    this.$message({
                        message: response.message,
                        type: 'success'
                    });  
                    this.dialogFormVisible = false;
                    this.loadData();
                }else{
                    this.$message({
                        message: response.message,
                        type: 'error'
                    });
                }
                this.loadData();
            }).catch(error => {
            this.$router.push({ path: '/login' });
            })


            
            this.dialogFormVisible = false;
            
            }).catch(() => {
            this.$message({
                type: 'info',
                message: '已取消删除'
            });          
            });
      },
      updateDevice(){
            this.fileList = [];     
           this.dialogFormVisible2 = true;
		   this.formtitle ="绑定设备";   
           this.subData = {}; 
          
            this.isEdit = true;
            this.subData = {
                 type:"10",
                 standards:"10",
                 state:"10"
                
            };

            this.paths = "";
            this.subData.userName = sessionStorage.getItem("loginName");
           this.subData.userId = sessionStorage.getItem("userId"); 
      },
     
      open(){
        // this.subData.fileUrl =  this.paths.substr(0,this.paths.length-1);


        // if(this.subData.classes.trim()=="" || this.subData.classes == null){
        //     this.$message({
        //         type: 'error',
        //         message: '分类不能为空'
        //     });         
        //     return false;
        // }
         
        // var uploadfile = document.getElementById("uploadfile"); 
        // this.subData.file = uploadfile.files[0];
        //alert(this.subData.file);
        // $("#formid").attr("action",$base_path+"/file/upload");
        // $("#formid").submit();
        // this.subData.communityId = sessionStorage.getItem("communityId");
        // alert(sessionStorage.getItem("communityId"));

       // this.subData.userName = this.userName;

        if(this.validate()==false){
            return;
        }

       if(this.formtitle!="修改"){
             RequestPost("/sysResident/add",this.subData).then(response => {
                if(response.code == '0000'){
                        this.$message({
                            message: response.message,
                            type: 'success'
                        });  
                        this.dialogFormVisible = false;
                        this.loadData();   
                }else{
                    this.$message({
                    type: 'error',
                    message:  response.message,
                }); 
                }
                        
            }).catch(error => {
                            this.$router.push({ path: '/login' });
                            
            })  

       }else{

            RequestPost("/sysResident/update",this.subData).then(response => {
                if(response.code == '0000'){
                        this.$message({
                            message: response.message,
                            type: 'success'
                        });  
                        this.dialogFormVisible = false;
                        this.loadData();   
                }else{
                    this.$message({
                    type: 'error',
                    message:  response.message,
                }); 
                }
                        
            }).catch(error => {
                            this.$router.push({ path: '/login' });
                            
            })  

       }
       
      },


     
    

	
	loadData(){

      //  this.page.userName = sessionStorage.getItem("loginName");
      //  this.page.state = '10';
       // this.files = fileUrls;    


        this.page.cardNo = this.remark;
        this.page.type = this.type;
        this.page.classes='10';

        if("adminA" == sessionStorage.getItem("loginName")){
            this.page.userId = "";
        }else{
            this.page.userId = sessionStorage.getItem("userId");
        }

       
        // this.page.standards = this.standards;
        // this.page.classes = this.classes;

        
        

		RequestGet("/sysResidentAnswer/findAll",this.page).then(response => {
						if(response.code == '0000'){
								 this.datalist = response.data;
								 this.total = response.page.totalCount; 
								 this.totalsize  = response.page.pageSize;
						 }
					
		}).catch(error => {
						 this.$router.push({ path: '/login' });
						
        }) 
        
        
        RequestGet("/sysClasses/findAllSelect","").then(response => {
						if(response.code == '0000'){
								 this.options1  = response.data;
						 }
					
		}).catch(error => {
						 this.$router.push({ path: '/login' });
						
		}) 
        
						
	
	},

  
    
 
    validate(){
        if(this.subData.name=="" || this.subData.name == null){
            this.$message({
                type: 'error',
                message: '姓名不能为空'
            });         
            return false;
        }
        
        if(this.subData.age=="" || this.subData.age == null){
            this.$message({
                type: 'error',
                message: '年龄不能为空'
            });         
            return false;
        }

        if(this.subData.cardNo=="" || this.subData.cardNo == null){
            this.$message({
                type: 'error',
                message: '住院号不能为空'
            });         
            return false;
        }


        if(this.subData.nation=="" || this.subData.nation == null){
            this.$message({
                type: 'error',
                message: '民族不能为空'
            });         
            return false;
        }

        if(this.subData.height1=="" || this.subData.height1 == null){
            this.$message({
                type: 'error',
                message: '身高不能为空'
            });         
            return false;
        }

        if(this.subData.weight1=="" || this.subData.weight1 == null){
            this.$message({
                type: 'error',
                message: '体重不能为空'
            });         
            return false;
        }

        if(this.subData.vocation=="" || this.subData.vocation == null){
            this.$message({
                type: 'error',
                message: '职业不能为空'
            });         
            return false;
        }

        if(this.subData.record=="" || this.subData.record == null){
            this.$message({
                type: 'error',
                message: '学历不能为空'
            });         
            return false;
        }

        if(this.subData.address=="" || this.subData.address == null){
            this.$message({
                type: 'error',
                message: '家庭住址不能为空'
            });         
            return false;
        }

        if(this.subData.phone=="" || this.subData.phone == null){
            this.$message({
                type: 'error',
                message: '联系号码不能为空'
            });         
            return false;
        }

        return true;

       

        


    },




	},
	//1
	created:function(){
		//3
        this.loadData();
        // var _self = this;
        // document.onkeydown = function(e){
        //     if(window.event == undefined){
        //         var key = e.keyCode;
        //     }else{
        //         var key = window.event.keyCode;
        //     }
        //     if(key == 13){
               
        //        alert(key);
        //        //_self.loginEnter('loginData');
        //     }
        // }
            
		
	
	},
    data() {
      return {
		total:0,     //数据的总数量
		totalsize:0,  //总的页数 = 总数量/每页显示的条数
        currentPage:1,
        options1:[],
        remark:"",
        classes:"",
        type:"",
        standards:"",
        userName:"",
		page:{
			pageSize:PageSize,   //一页显示的条数
            userName:''
		},
        datalist:[],
        formtitle:"",
        fileList: [],
        paths:"",
        files:"",
        classes1List:[],  //医生
        classes2List:[],  //患者
		listLoading: false,
        dialogFormVisible:false,
        dialogFormVisible2:false,
        disabledBtn:false,

        manyAnswer:[],
        sunCount:0,
      
        subData:{
            type:"",
            name:""
        },

      };
    }
  }
</script>
<style>
.customWidth{
      width:180%;
      margin-left:-40%;
      margin-top:-5%;
     
}        
	/* .el-dialog--small {
		 width: 60%; 
	} */
    .span {
        position:relative;
        padding:8px;
    }
    .tip {
        display:block;
        background:#f00;
        border-radius:50%;
        width:8px;
        height:8px;
        top:12px;
        right:0px;
        position:absolute;
    }


    .height1{
        /* height:30px; */
       margin-bottom: -10px;
    }


    #table1{
      /* border:1px solid #ccc; */
      margin:20px 0px;
    }


    .checkText{
        border-bottom:solid 1px #ccc;
        text-align: center;
         width:12.5%;
        color:#000;
    }

    .checkTitle{
         text-align: center;
         width:12.5%;

    }
     #table1{
         margin-left:-4%;
     }

     #table1 tr{
      height: 35px;
     }
    #table1 tr td{
        font-size:17px;
         margin:20px 0px;
        /* text-align: left; */
         /* border:1px solid #ccc; */
        /* border:1px solid #ccc; */
    }
</style>
