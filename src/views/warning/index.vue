<template>
	<div class="titles">
		<div class="screen">
			关键字：
			<el-input v-model="qhkeyword" placeholder="编号/内部编号" class="select" size='mini'/>
			类型：<el-select v-model="style_value" placeholder="请选择" class="select" size='mini'>
				<el-option v-for="item in style_list" :key="item.value" :label="item.label" :value="item.value" />
			</el-select>
			处置状态：<el-select v-model="cz_value" placeholder="请选择" class="select" size='mini'>
				<el-option v-for="item in cz_list" :key="item.value" :label="item.label" :value="item.value" size='mini'/>
			</el-select>
			日期：
			<el-date-picker v-model="timevalue" type="daterange" range-separator="至"
      start-placeholder="开始日期" end-placeholder="结束日期" size='mini' class='selectDate' />
      <span style="float: right;margin-right: 1.5%;">
          <el-button class="btn" type="primary" icon="el-icon-search" size="mini" round>搜索</el-button>
      </span>
		</div>
		<div class="container_table">
			<div class="table_headr">报警记录</div>
			<el-table ref="multipleTable" :data="tableData.slice((currentPage-1)*pagesize,currentPage*pagesize)" border stripe
			 style="width: 99%;font-size: 13px;" :default-sort="{prop: 'date', order: 'descending'}" size='mini'>
				<el-table-column prop="bh" sortable label="设备编号" align="center" width="170" />
				<el-table-column prop="nbbh" sortable label="内部编号" align="center" />
				<el-table-column prop="bjms" label="报警描述" align="center" />
				<el-table-column prop="czzt" label="处置状态" align="center">
					<template slot-scope="scope">
						<el-tag :type="scope.row.czzt === '已处置' ? 'primary' : 'danger'" disable-transitions  size='mini'>{{ scope.row.czzt }}</el-tag>
					</template>
				</el-table-column>
				<el-table-column prop="cssj" label="产生时间" align="center" />
				<el-table-column label="操作" width="180" align="center">
					<template slot-scope="scope">
						<el-button type="primary" size="mini" round  @click="set_up(scope.$index, scope.row)" >操作记录</el-button>
						<el-button type="danger" size="mini" @click="xiugai(scope.$index, scope.row)" round  >处置</el-button>
					</template>
				</el-table-column>
			</el-table>
			<el-pagination class="fy" layout="total,prev, pager, next" :total="total" background @current-change="current_change" />
			<el-dialog title="处置" :visible.sync="dialogVisible" width="35%">
				<div>
					<div style="margin-bottom: 0.625rem;">设备编号：{{ bh }}</div>
					<div>
						处置选项：<el-select v-model="qyvalue" placeholder="请选择" size='mini'>
							<el-option v-for="item in qycompany" :key="item.value" :label="item.label" :value="item.value" size='mini'/>
						</el-select>
					</div>
				</div>
				<span slot="footer" class="dialog-footer">
					<el-button @click="dialogVisible = false" size='mini'>取 消</el-button>
					<el-button type="primary" @click="dialogVisible = false" size='mini'>确 定</el-button>
				</span>
			</el-dialog>
		</div>
	</div>
</template>

<script>
	export default {
		data() {
			return {
				dialogVisible: false,
				timevalue: '',
				total: 1000, // 默认数据总数
				pagesize: 10, // 每页的数据条数
				currentPage: 1, // 默认开始页面
				istag: true,
				tableData: [{
						bh: 'QHW001G210290201',
						nbbh: 'qh001',
						bjms: '脉冲阀异常',
						czzt: '未处置',
						cssj: '2020-04-19 09:25:45'
					},
					{
						bh: 'QHW001G210290202',
						nbbh: 'qh002',
						bjms: '脉冲阀异常',
						czzt: '未处置',
						cssj: '2020-04-19 09:25:45'
					}
				],
				cz_list: [{
					value: '选项0',
					label: '全部'
				}, {
					value: '选项1',
					label: '已处置'
				}, {
					value: '选项2',
					label: '未处置'
				}],
				qycompany: [{
					value: '选项1',
					label: '潜合测试'
				}, {
					value: '选项2',
					label: '客户体验'
				}],
				company_list: [{
					value: '选项0',
					label: '全部'
				}, {
					value: '选项1',
					label: '潜合测试'
				}, {
					value: '选项2',
					label: '客户体验'
				}],
				style_list: [{
					value: '选项0',
					label: '全部'
				}, {
					value: '选项1',
					label: '脉冲阀'
				}, {
					value: '选项2',
					label: '提升阀'
				}],
				status_list: [{
					value: '选项0',
					label: '全部'
				}, {
					value: '选项1',
					label: '在线'
				}, {
					value: '选项2',
					label: '离线'
				}],
				company_value: '',
				style_value: '',
				status_value: '',
				cz_value: '',
				qhkeyword: '',
				bh: '',
				qyvalue: ''
			}
		},
		created: function() {
			this.total = this.tableData.length
		},
		methods: {
			xiugai(index, row) {
				this.dialogVisible = true
				this.bh = row.bh
			},
			current_change: function(currentPage) {
				this.currentPage = currentPage
			},
     set_up(index, row) {
     	this.$router.push({
     		path: 'set_up',
     		query: {
     			key: row
     		}
     	})
     },
		}
	}
</script>

<style scoped="scoped">
	.screen {
			font-size: 0.875rem;
	    color: gray;

		}
	.table_headr{
	    height: 2rem;
	    line-height: 2rem;
	    color: white;
	    background-color: #409EFF ;
      width: 99%;

	  }
		.select {
			width: 10rem;
      margin-right: 1%;
		}

    .selectDate {
    	width: 15rem;

      margin-right: 1%;
    }

		.fy {
			text-align: right;
			margin-top: 0.625rem;
		}

		.titles {
		  width: 98%;
			margin-left: 1%;
	    margin-top: -0.8rem;
			height: 100%;
			margin-bottom: 1.25rem;
			font-size: 0.875rem;
		}

	.btn {
		margin-left: 0.5rem;
		margin-bottom: 0.625rem;

	}

	.container_table {
		clear: both;
	}

	@media screen and (max-width: 1024px) {
	.titles {
	  font-size: 0.875rem;
	}

	.select {
	  width:6.7rem;

	  margin-right: 0.5rem;
	}
  .selectDate {
  	width: 21%;
    margin-right: 1%;
    padding: 0.2rem;


  }
		.btn {
			margin-top: 0.625rem;
		}
	}
</style>
