<template>
  <div id="building">
        <div class="login_title">
          <h2>Blockchain System</h2>
        </div>

    <el-tabs type="border-card" style="width: 90%;margin: auto">

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
              sortable
              prop="node"
              label="total_nodes"
            >
            </el-table-column>
          </el-table>

        </el-form>

      </el-tab-pane>
      <el-tab-pane label="New Transactions">

        <el-form  :model="txForm" label-width="80px" style="width: 95%;margin: auto" :rules="txFormRules" ref="txFormRules" prop="form">
          <el-form-item label="sender" prop="sender">
            <el-input v-model="txForm.sender" ></el-input>
          </el-form-item>
          <el-form-item label="recipient" prop="recipient">
            <el-input v-model="txForm.recipient" ></el-input>
          </el-form-item>
          <el-form-item label="amount" prop="amount">
            <el-input v-model="txForm.amount" ></el-input>
          </el-form-item>
          <el-form-item>
            <el-button type="primary" @click="addTx">Create</el-button>
          </el-form-item>
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
              prop="id"
              label="id"
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

            <el-table-column
              prop="merkle_root"
              label="merkle_root">
            </el-table-column>

          </el-table>

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
            prop="id"
            label="id"
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

          <el-table-column
            prop="merkle_root"
            label="merkle_root">
          </el-table-column>

        </el-table>

      </el-tab-pane>


    </el-tabs>

  </div>

</template>

<script>
export default {
  data() {
    return {
      txForm: {
        sender: '',
        recipient: '',
        amount:'',

      },
      txFormRules: {
        sender: [{
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
        recipient: [{
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
        amount: [{
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
        id:'',
        transactions : '',
        timestamp : '',
        previous_hash : '',
        current_hash: '',
        difficulty:'',
        proof : '',
        merkle_root:'',
      },

      neighbor_from:'',
      neighbor_to:'',
      neighborForm:{
        node:''
      },

      nodeData: [],
      nodeDataForm:'',
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
  // created(){
  //
  //   this.interval = setInterval(() => this.mineBlock(),2000);
  // },
  methods: {
    addTx() {
      this.$refs.txFormRules.validate((valid) => {
        if (!valid) return;
        this.$axios.post("/api/transactions/new", this.qs.stringify(this.txForm), {
          headers:{
            "Content-Type": "application/json;charset=utf8"
          }
        }).then((
          res) => {
          console.log(res)
          if (res.status === 201) {
            this.$message.success("Created successfully!"+res.data.message);
          }
          else{
            this.$message.error("Failed to create");
          }
        })
      })
    },
    // ??????????????????blocks???
    showBlockchain(){
      this.$axios.get("/api/getblocks").then((
        res) => {
        console.log(res)
        // this.$message.success(res.status);
        if (res.status === 200) {
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
          if (res.status === 200) {
            // if(this.mineData.length === 1) {
            //   this.mineData.pop();
            // }
            // this.showBlockchain();
            this.mineData.push(res.data.block);
            this.$message.success(res.data.message);
          } else {
            this.$message.error("????????????");
          }
        })
    },
    nodeRegister(){
      this.nodeRegisterForm.nodes=this.nodeRegisterInfo
      this.$axios.post("/api/nodes/register", this.qs.stringify(this.nodeRegisterForm),{
          headers:{
            "Content-Type": "application/json;charset=utf8"
          }
        }
      ).then((
      res) => {
        console.log(res)
        // console.log(22)
        this.$message.success(res.data.total_nodes)
        if (res.status === 201 ) {
          this.nodeData = res.data.total_nodes;
        }
        else{
          this.$message.error("fail");
        }}
      )
    },
    // addNeighbor(){
    //   this.neighborForm.node = this.neighbor_from;
    //   this.$axios.post("/api/afddneighbor", this.qs.stringify(this.neighborForm), {
    //       headers: {
    //         "Content-Type": "application/json;charset=utf8"
    //       }
    //     }
    //   ).then((
    //     res) => {
    //     console.log(res)
    //     if (res.data.code === 200 ) {
    //       this.$message.success(res.data.message);
    //     }
    //     else{
    //       this.$message.error("fail");
    //     }}
    //   )
    // }
  }
}

</script>

<style scoped>
.login_title {
  /* ???????????????????????? */
  text-align:center;
  font-size: 40px;
  color:white;
}

.login_context {
  /* ?????? */
  width: 450px;
  /* ?????? */
  height: 300px;
  /* ????????? */
  background: #fff;
  /* ???????????? */
  position: absolute;
  /* ??????????????????????????? */
  top: 50%;
  /* ??????????????????????????? */
  left: 50%;
  /* ?????????????????? */
  transform: translate(-50%, -50%);
  /* ???????????????????????? */
  border-radius: 20px;
  /* ?????? */
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
