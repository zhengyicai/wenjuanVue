<template> 
 <div class="block">   
	 
    <el-col :span="20" class="toolbar" style="padding-bottom: 0px;">
			<el-form :inline="true">
			
              

                   
                    
					<el-select v-model="solt">
                  <el-option value="" label="">全部</el-option>
                  <el-option value="10" label="术前">术前</el-option>
                  <el-option value="20" label="术后">术后</el-option>
                  
                          
					</el-select>                   
                   
			


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
					<el-button type="primary" @click="add()">添加</el-button>
				</el-form-item>

                <!-- <el-form-item  style="text-align:right">
					<el-button type="primary" @click="updateDevice()">绑定设备</el-button>
				</el-form-item> -->
			</el-form>
		</el-col>
    
        
		<el-table :data="datalist" highlight-current-row v-loading="listLoading" style="width: 100%;">
      	<el-table-column  label="问卷类型" min-width="120" sortable>
				<template slot-scope="scope">{{ scope.row.solt=='10'?"术前":"术后"}}</template>
			</el-table-column>
			
			<el-table-column prop="title" label="标题" width="400" sortable>
			</el-table-column>
		
			<el-table-column  label="创建时间" min-width="150">
				<template slot-scope="scope">{{ scope.row.createTime | moment('YYYY-MM-DD') }}</template>
			</el-table-column>
       <el-table-column prop="sort" label="排序" width="150" sortable>
			</el-table-column>
      
		
			<el-table-column  label="状态" min-width="150">
				<template slot-scope="scope">{{ state(scope.row.state)}}</template>
			</el-table-column>
       <el-table-column prop="remark" label="分值" width="150" sortable>
			</el-table-column>

      	<el-table-column  label="面对人群" min-width="120" sortable>
				<template slot-scope="scope">{{ scope.row.type=='10' || scope.row.type=='30'?"患者":"医生"}}</template>
			</el-table-column>

      
     
			
			<el-table-column label="操作" min-width="200">
				<template scope="scope">
				 <el-button size="small" type="primary"  @click="edit(scope.$index,scope.row)">编辑</el-button>
			
				</template>
			</el-table-column>
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

        <el-dialog   :title="formtitle" :visible.sync="dialogFormVisible" >
			<el-form ref="subData" :model="subData" label-width="100px" @submit.prevent="onSubmit" style="margin:20px;">
                    <el-form-item label="*标题">
                      <el-input v-model="subData.title"  placeholder="请输入标题"></el-input>
                    </el-form-item>
                    
                   
                    <el-form-item label="*目录" >

                   <el-col :span="24" style="text-align:center">
                      <el-col :span="11">
                          <el-select  v-model="subData.classesId" @change="selectCity(subData.classesId)" placeholder="一级目录">
                               <el-option :label="item.classes" :key="item.id" :value="item.id" v-for="item in provinces">{{item.classes}}</el-option>
                          </el-select>
                      </el-col>
                      <el-col :span="2">
                        &nbsp;
                      </el-col>  
                      <el-col :span="11">
                          <el-select  v-model="subData.classesId2" placeholder="二级目录" @change="selectCity1(subData.classesId2)">
                               <el-option :label="item1.classes" :key="item1.id" :value="item1.id" v-for="item1 in provincesTwo">{{item1.classes}}</el-option>
                          </el-select>
                      </el-col>
                   </el-col>
                   </el-form-item>
                    <el-form-item label="*排序">
                      <el-input v-model="subData.sort"  placeholder="请输入排序"></el-input>
                    </el-form-item>
                      
                     
                    
                     <el-form-item label="*分值">
                      <el-input v-model="subData.remark"  placeholder="请输入分值"></el-input>
                    </el-form-item>


                    <el-form-item label="面对人群">
                        <el-radio-group  name="type" v-model="subData.type">
                            <el-radio  label="10">患者</el-radio>
                            <el-radio label="20">医生</el-radio>
                        </el-radio-group>
                    </el-form-item>


                    <el-form-item label="问卷类型">
                        <el-radio-group  name="solt" v-model="subData.solt">
                            <el-radio  label="10">术前</el-radio>
                            <el-radio label="20">术后</el-radio>
                        </el-radio-group>
                    </el-form-item>



                    

                    
                    <el-form-item label="*选项">
                      <el-button type="primary"  @click="addEl">新增选项</el-button>
                    </el-form-item>
                    <el-form-item>
                        <el-row>
                         
                            <el-col :span="24"  :key="list.name" v-for="(list,index) in lists">
                                <el-row :span="24"   class="margins">
                                  
                                    <el-col :span="1">
                                        {{index+1}}
                                    </el-col>
                                    <el-col :span="18">
                                        <el-col :span="24">
                                          <el-col :span="14">
                                               <el-input type="text" placeholder="请输入内容" v-model="list.answer"/>
                                          </el-col>
                                          <el-col :span="10">
                                         <el-input  type="text" placeholder="请输入分值" v-model="list.remark"/>
                                           </el-col>
                                        </el-col>
                                    </el-col>
                                    <el-col :span="5">
                                        <el-button type="danger" @click="del(index)">删除</el-button>
                                    </el-col>
                                </el-row>
                            </el-col>
                        </el-row>


                    </el-form-item>

                    
			</el-form>	
			<div slot="footer" class="dialog-footer">
				<el-button @click="dialogFormVisible = false">取 消</el-button>
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

      add(){
       this.dialogFormVisible = true;
		   this.formtitle ="新增参数";   
       this.subData = {}; 
       this.lists = [];
       


           
            this.subData = {
                classes:"",
                sort:"",
                level:"1",
                remark:"",
                type:"10"
            };
            //this.subData.classesId2 = "";



      },
      
      selectCity(obj){

        	RequestGet("/sysClasses/findLevelTwo",{parentId:obj}).then(response => {
                  if(response.code == '0000'){
                      this.provincesTwo = response.data;
                    
                  }
                
          }).catch(error => {
                  this.$router.push({ path: '/login' });
                  
          })  

        
		
	    	
      },

      selectCity1(obj){
        //alert(obj);

        
		
	    	
      },
    
       edit(index, rows){


         RequestGet("/sysClasses/findLevelTwo",{parentId:rows.classesId}).then(response => {
                  if(response.code == '0000'){
                      this.provincesTwo = response.data;
                    
                  }
                
          }).catch(error => {
                  this.$router.push({ path: '/login' });
                  
          }) 

            this.dialogFormVisible = true;
		         this.formtitle ="修改参数";   
             this.subData = rows;
             this.lists = rows.answer;
        //alert("asdf");


      },
      open(){
          if(this.formtitle == '新增参数'){


            if(this.subData.classesId==""){
                 this.$message({
                    message: "请选择目录",
                    type: 'error'
                });  
                return;

            }

            
            if(this.subData.sort==""){
                 this.$message({
                    message: "请输入排序",
                    type: 'error'
                });  
                return;

            }
           

          //   alert(this.lists);
          //  return;
           this.subData.answer = this.lists;

          //this.subData.type="10";

            RequestPost("/sysQuestions/add",this.subData).then(response => {
						
						//this.logining = false; 
						if(response.code=='0000'){
							this.$message({
								message: response.message,
								type: 'success'
							});  
							this.dialogFormVisible = false;
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

          }else{
              RequestPost("/sysQuestions/update",this.subData).then(response => {
						
						//this.logining = false; 
						if(response.code=='0000'){
							this.$message({
								message: response.message,
								type: 'success'
							});  
							this.dialogFormVisible = false;
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
          }
          
          
      },  
	  deleteRow(index, rows) {
       this.$confirm('确认删除, 是否继续?', '提示', {
            confirmButtonText: '确定',
            cancelButtonText: '取消',
            type: 'warning'
            }).then(() => {
            // this.$message({
            //     type: 'success',
            //     message: '删除成功!'
            // });

            RequestPost("/parameter/delete",rows).then(response => {
						
						//this.logining = false; 
                if(response.code=='0000'){
                    this.$message({
                        message: response.message,
                        type: 'success'
                    });  
                    this.dialogFormVisible = false;
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
     

	
	loadData(){

    this.page.solt= this.solt;
		RequestGet("/sysQuestions/findAll",this.page).then(response => {
						if(response.code == '0000'){
								 this.datalist = response.data;
								 this.total = response.page.totalCount; 
								 this.totalsize  = response.page.pageSize;
						 }
					
		}).catch(error => {
						 this.$router.push({ path: '/login' });
						
    })  
    

    	RequestGet("/sysClasses/findLevelOne",{}).then(response => {
						if(response.code == '0000'){
								 this.provinces = response.data;
							
						 }
					
		}).catch(error => {
						 this.$router.push({ path: '/login' });
						
		})  
        
						
	
	},

  
    
    /**
    * 选择设备类型
    */
    selectEquType(){
        if(this.subData.equipmentType === '30'){
            this.isEdit = false;
        }else{
            this.isEdit = true;
        }
    },

    // /**
    // * 选择小区
    // */
    selectCommunity(selectIdx){
        
        this.communityId = selectIdx;
        this.subData.communityId = selectIdx;
        this.buildingId = '';
        this.subData.buildingId = "";
        this.buildings = [];
        this.unitNo = '';
        this.loadBuildings();
    },

    // /**
    // * 选择楼栋
    // */
    selectBuild(selectIdx){
        //this.buildingId = this.buildings[selectIdx].value;
        this.buildingId = selectIdx;
        this.subData.buildingId = this.buildingId;
        this.subData.unitName = '';
        this.units = [];
        this.unitNo = '';
        this.loadUnits();
    },

    // /**
    // * 选择单元
    // */
    selectUnit(selectIdx){
        this.unitNo = selectIdx;
        this.subData.unitName = this.unitNo;
    },

    /**
    * 加载小区
    */
    loadCommunitys(){

        RequestGet("/equipment/findCommunitys").then(response => {
          
						if(response.code == '0000'){
								this.communitys = response.data;
                                
                                
						 }
					
        }).catch(error => {
                this.$router.push({ path: '/login' });
						
		})  
      
    },

    /**
    * 加载楼栋
    */
    loadBuildings(){

        RequestGet("/equipment/findBuildings",{
                communityId:this.communityId
            }).then(response => {
          
						if(response.code == '0000'){
								 this.buildings = response.data;
                                 if(this.buildings.length>0){
                                     this.subData.buildingId = this.buildings[0].value;
                                 }
                                 
                                // for(let obj in this.buildings){
                                //     this.selectBuild(0);
                                //     break;
                                // }
						 }
					
        }).catch(error => {
                this.$router.push({ path: '/login' });
						
		})  
       
    },
    addEl: function() {
        let cope = {
            name: "",
        }
        this.lists.push(cope);
    },
    del: function(index) {
        this.lists.splice(index, 1);
    },

    // /**
    // * 加载单元
    // */
    loadUnits(){

         RequestGet("/equipment/findUnits",{
                buildingId:this.buildingId
            }).then(response => {
          
						if(response.code == '0000'){
								  this.units = response.data;
                                   if(this.units.length>0){
                                     this.subData.unitName = this.units[0].value;
                                 }
                                // for(let obj in this.buildings){
                                //     this.selectBuild(0);
                                //     break;
                                // }
						 }
					
        }).catch(error => {
                this.$router.push({ path: '/login' });
						
		})  
        
    },




	},
	//1
	created:function(){
		//3
		this.loadData();
		
	
	},
    data() {
      return {
		total:0,     //数据的总数量
		totalsize:0,  //总的页数 = 总数量/每页显示的条数
		currentPage:1,
		page:{
			pageSize:PageSize,   //一页显示的条数
            criteria:''
    },
    solt:"",
    datalist:[],
    provinces:[],
    provincesTwo:[],
    listLoading: false,
    lists: [{
       answer: "",
    }],
		form:{},
        subData:{},
        isEdit:true, //是否禁用
        dialogFormVisible:false,
        communitys:[],
        buildings:[],
        units:[],
        communityId:"",
        buildingId:"",
        unitNo:"",
        formtitle:"",
         options: [{
          value: '10',
          label: '管理机'
        }, {
          value: '20',
          label: '围墙机'
        }, {
          value: '30',
          label: '单元门口机'
        }],

      };
    }
  }
</script>
<style>
	.el-dialog--small {
		 width: 30%; 
	}
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
</style>
