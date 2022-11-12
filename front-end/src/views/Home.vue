<template>
  <div id="building">
        <div class="login_title">
          <h2>Blockchain System</h2>
        </div>

    <el-tabs type="border-card" style="width: 90%;margin: auto">
      <el-tab-pane label="New Transactions">

        <el-form  :model="txForm" label-width="80px" style="width: 95%;margin: auto" :rules="txFormRules" ref="txFormRules" prop="form">
          <el-form-item label="sender" prop="Sender">
            <el-input v-model="txForm.Sender" ></el-input>
          </el-form-item>
          <el-form-item label="recipient" prop="Recipient">
            <el-input v-model="txForm.Recipient" ></el-input>
          </el-form-item>
          <el-form-item label="amount" prop="Amount">
            <el-input v-model="txForm.Amount" ></el-input>
          </el-form-item>
          <el-form-item>
            <el-button type="primary" @click="addTx">Create</el-button>
          </el-form-item>
        </el-form>


      </el-tab-pane>

      <el-tab-pane label="Show Blockchain">

          <el-form style="width: 95%;margin: auto">
            <el-form-item lable="search" >
            <el-col :span="15">
              <el-form-item label="">
                <el-input v-model="shownode" placeholder="Please enter the node"></el-input>
              </el-form-item>
            </el-col>

              <el-col :span="3">
                <el-button type="primary" @click="showBlockchain">show</el-button>
              </el-col>

            </el-form-item>


          </el-form>

        <el-table
          :data="blockChainData"
          height="400"
          border
          style="width: 100%">
          <el-table-column
            prop="index"
            label="index"
           >
          </el-table-column>
          <el-table-column
            prop="timestamp"
            label="timestamp"
            >
          </el-table-column>
          <el-table-column
            prop="previous_hash"
            label="previous_hash">
          </el-table-column>
          <el-table-column
            prop="current_hash"
            label="current_hash">
          </el-table-column>
          <el-table-column
            prop="difficulty"
            label="difficulty">
          </el-table-column>

          <el-table-column
            prop="proof"
            label="proof">
          </el-table-column>

          <el-table-column
            prop="transactions"
            label="transactions">
          </el-table-column>

        </el-table>





      </el-tab-pane>

      <el-tab-pane label="Add Neighbor">
        <el-col :span="2">add:</el-col>
        <el-col :span="8">
            <el-input v-model="neighbor_from" placeholder="Please enter the node"></el-input>
        </el-col>
        <el-col :span="2">to</el-col>
        <el-col :span="8">
            <el-input v-model="neighbor_to" placeholder="Please enter the node"></el-input>
        </el-col>
        <el-col :span="3">
          <el-button type="primary" @click="addNeighbor">Add</el-button>
        </el-col>

        </el-tab-pane>

      <el-tab-pane label="Register Node">
        <el-form style="width: 95%;margin: auto">
          <el-form-item lable="" >
            <el-col :span="15">
              <el-form-item label="">
                <el-input v-model="nodeRegisterInfo" placeholder="Please enter the node"></el-input>
              </el-form-item>
            </el-col>

            <el-col :span="3">
              <el-button type="primary" @click="nodeRegister">Register</el-button>
            </el-col>
          </el-form-item>

          <el-table
            :data="nodeData"
            height="400"
            border
            style="width: 100%">
            <el-table-column
              prop="node"
              label="total node"
            >
            </el-table-column>
          </el-table>

        </el-form>

      </el-tab-pane>


      <el-tab-pane label="Mine">
        <el-form style="width: 95%;margin: auto">
          <el-form-item lable="" >
            <el-col :span="15">
              <el-form-item label="">
                <el-input v-model="info" placeholder="Please enter the node"></el-input>
              </el-form-item>
            </el-col>

            <el-col :span="3">
              <el-button type="primary" @click="mineBlock">mine</el-button>
            </el-col>

          </el-form-item>
          <el-table
            :data="mineData"
            height="400"
            border
            style="width: 100%">
            <el-table-column
              prop="index"
              label="index"
            >
            </el-table-column>
            <el-table-column
              prop="timestamp"
              label="timestamp"
            >
            </el-table-column>
            <el-table-column
              prop="previous_hash"
              label="previous_hash">
            </el-table-column>
            <el-table-column
              prop="current_hash"
              label="current_hash">
            </el-table-column>
            <el-table-column
              prop="difficulty"
              label="difficulty">
            </el-table-column>

            <el-table-column
              prop="proof"
              label="proof">
            </el-table-column>

            <el-table-column
              prop="transactions"
              label="transactions">
            </el-table-column>

          </el-table>

        </el-form>


      </el-tab-pane>

    </el-tabs>

  </div>

</template>

<script>
export default {
  data() {
    return {
      txForm: {
        Sender: '',
        Recipient: '',
        Amount: '',
      },
      txFormRules: {
        Sender: [{
          required: true,
          message: "Please enter sender",
          trigger: "blur"
        },
          {
            min: 3,
            max: 18,
            message: "Length between 3 and 18 characters",
            trigger: "blur",
          },
        ],
        Recipient: [{
          required: true,
          message: "Please enter recipient",
          trigger: "blur"
        },
          {
            min: 3,
            max: 18,
            message: "Length between 3 and 18 characters",
            trigger: "blur",
          },
        ],
        Amount: [{
          required: true,
          message: "Please enter amount",
          trigger: "blur"
        },
        ],

      },
      value: '',
      info:'',
      searchForm:{
        Info:'',
        Method:''
      },
      shownode:'',

      blockChainData: [],
      blockChainInfo:{
        index:'',
        transactions : '',
        timestamp : '',
        previous_hash : '',
        current_hash: '',
        difficulty:'',
        proof : ''
      },

      neighbor_from:'',
      neighbor_to:'',
      neighborForm:{
        node:''
      },

      nodeData: [],
      mineData:[],

      recordInfo:{
        PatientName: '',
        In_time: '',
        Out_time: '',
        Fee: '',
        RecordId:''
      },

      nodeRegisterInfo: '',
      nodeRegisterForm: {
        nodes:''
      }


    }
  },
  methods: {
    addTx() {
      this.$refs.txFormRules.validate((valid) => {
        if (!valid) return;
        this.$axios.post("/api/transactions/new", this.qs.stringify(this.txForm)).then((
          res) => {
          console.log(res)
          if (res.data.code === 201) {
            this.$message.success("Created successfully!"+res.data.message);
          }
          else{
            this.$message.error("Failed to create");
          }
        })
      })
    },
    // 从某节点得到blocks？
    showBlockchain(){
      this.$axios.get("/api/getblocks").then((
        res) => {
        console.log(res)
        if (res.data.code === 200) {
          this.$message.success(res.data.message);
          this.blockChainData = res.data.blocks;
        }
        else{
          this.$message.error("Fail");
        }
      })
    },

    mineBlock(){
      this.$axios.get("/api/mine").then((
        res) => {
        console.log(res)
        if (res.data.code === 200) {
          this.mineData = res.data;
          this.$message.success(res.data.message);
        }
        else{
          this.$message.error("搜索失败");
        }
      })
    },
    nodeRegister(){
      this.nodeRegisterForm.nodes=this.nodeRegisterInfo
      this.$axios.post("/nodes/register", this.qs.stringify(this.nodeRegisterForm.nodes)).then((
      res) => {
        console.log(res)
        if (res.data.code === 200 ) {
          this.nodeData = res.data.total_nodes;
        }
        else{
          this.$message.error("fail");
        }}
      )
    },
    addNeighbor(){
      this.neighborForm.node = this.neighbor_from;
      this.$axios.post("/nodes/register", this.qs.stringify(this.neighborForm)).then((
        res) => {
        console.log(res)
        if (res.data.code === 200 ) {
          this.$message.success(res.data.message);
        }
        else{
          this.$message.error("fail");
        }}
      )
    }
  }
}

</script>

<style scoped>
.login_title {
  /* 字体水平左右居中 */
  text-align:center;
  font-size: 40px;
  color:white;
}

.login_context {
  /* 宽度 */
  width: 450px;
  /* 高度 */
  height: 300px;
  /* 背景色 */
  background: #fff;
  /* 属性定位 */
  position: absolute;
  /* 属性定位，顶部占比 */
  top: 50%;
  /* 属性定位，左侧占比 */
  left: 50%;
  /* 水平垂直居中 */
  transform: translate(-50%, -50%);
  /* 四个角的圆角角度 */
  border-radius: 20px;
  /* 阴影 */
  box-shadow: 0 0 5px 2px #ddd;
}
#building{
  background:url("../assets/back.jpeg");
  width:100%;
  height:100%;
  position:fixed;
  background-size:100% 100%;
}
</style>
