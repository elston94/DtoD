<template>
<div>
  <br>
  <swiper-slide>
  <div class="item" @click="getItemDetail()">
    <div
      class="imgBox"
      :style="style"
    >
      <!-- <img
        v-for="(stack, idx) in imgs"
        :key="idx"
        id="stackImg"
        :src="require(`@/assets/stacks/${stack}.png`)"
        alt="img"
      > -->
    </div>
    <div class="item-title">
      <div v-for="(item,idx) in getTitle" :key="idx">{{ item }}</div>
    </div>
    <b-modal
      ref="detail"
      centered
      size="lg"
      hide-footer 
      hide-header
    >
      <MyFreeDetail
        :item_pk = this.item.cboardId
        @update-modal-open="updateModalOpen"
      />
    </b-modal>
    <b-modal
      ref="updateItem"
      centered
      size="lg"
      hide-footer 
      hide-header
    >
      <MyFreeUpdate
        :itempk="this.item_pk"
        @update-fin="updateFin"
        @close-modal="closeModal"
      />
    </b-modal>
  </div>
  </swiper-slide>
  <br>
</div>
</template>

<script>
import MyFreeDetail from './MyFreeDetail.vue'
import MyFreeUpdate from "./MyFreeUpdate.vue"
import { api } from '../../../../../api.js'
import axios from 'axios'

export default {
  name: 'Item',
  components: {
    MyFreeDetail,
    MyFreeUpdate
  },
  props: {
    item: Object,
  },
  data(){
    return{
      // imgs: null,
      imgPath: '',
      item_pk: 0,

      style: {
        backgroundColor: this.imgPath
      }
    }
  },
  computed: {
    getTitle: function() {
      const t = this.item.cboardTitle
      const temp = t.split(' ')
      let res = []
      let tp = ''
      for(let i = 0; i < temp.length; i++){
        if(tp.length + temp[i].length < 28 ){ 
          tp += ' ' + temp[i]
        }else{
          res.push(tp)
          tp = temp[i]
        }
      }
      res.push(tp)
      return res
    },
  },
  methods: {
    updateModalOpen(e) {
      this.getItemPk(e)
      // console.log(this.item_pk)
      this.$refs['detail'].hide()
      this.$refs['updateItem'].show()
    },
    
    updateFin(){
      this.$refs['updateItem'].hide()
      // this.$router.go();
      this.$refs['detail'].show()
    },
    closeModal(){
      this.$refs['updateItem'].hide()
      this.$refs['detail'].show()
    },
    getItemPk(e) {
      this.item_pk = e
      return this.item_pk
    },

    getItemDetail() {
      this.$refs['detail'].show()
    },
  },
  created() {
    const itempk = this.item.cboardId
    const token = localStorage.getItem('jwt')
    axios({
      url:  api.GET_FREE_BOARD_DETAIL + `${itempk}`,
      method: 'GET',
      headers: {
        Authorization: 'Bearer ' + token
      },
    }).then((res)=>{
      // console.log(res.data)
      // // db??? ????????? item??? ???????????? ????????????
      // let stacks = res.data.cboardTechstack
      // // ????????? ??????
      // let result = stacks.split(',')
      // // console.log(result.length)

      // // ????????? 4??? ???????????? 3?????? ?????? ??? ????????? ??? ??????
      // if(result.length >= 4){
      //   console.log(result.slice(0,3))
      //   this.imgs = result.slice(0,3)
      // }else{
      //   this.imgs = result
      // }
      this.imgPath = res.data.cboardImg
      this.style.backgroundColor = res.data.cboardImg
      // console.log(this.imgPath)
      // console.log(this.style.backgroundColor)
    }).catch((err)=>{
      console.error(err)
    })
  }
}
</script>

<style scoped>
.item{
  width: 13vw;
  height: 27vh;
  cursor: pointer;
  /* border: 1px solid; */
  border-radius: 2rem;
  box-shadow: 5px 5px 5px rgb(122, 122, 122);
  background-color: white;
  margin: auto;
}

.imgBox{
  display: flex;
  border-radius: 2rem 2rem 0 0;
  /* background-color: ; */
  flex-direction: row-reverse;
  justify-content: space-evenly;
  align-items: center;
  flex-wrap: nowrap;
  height: 40%;
}

#stackImg {
  width: 5vh;
}

.item .item-title {
  text-align: center;
  font-size: 0.78vw;
  margin: 2.5vh 0 0 0;
  font-family: 'Epilogue', sans-serif;
}

</style>