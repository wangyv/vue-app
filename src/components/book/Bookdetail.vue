<template>
    <div @click="handler">
        <ul>
            <li v-for="(detail,index) in bookDetails" :key="index" class="book-detail clearfix">
                <div class="detail-img">
                    <img :src="detail.img" :alt="detail.sub1">
                </div>
                <div class="detail-info">
                    <span v-text="detail.reading"></span>
                    <p class="detail-name">{{detail.title}}</p>
                    <p class="detail-star">作者：{{detail.sub1}}</p>
                    <p>类型：{{detail.catalog}}</p>
                    <p class="detail-show">{{detail.sub2}}</p>
                </div>
            </li>
        </ul>
        <div class="loading" v-show="isLoading">
            <img src="../../assets/img/loading.gif" alt="">
        </div>
    </div>
</template>

<script>
import axios from 'axios';
export default {
    data(){
        return {
            bookDetails:[],
            isLoading:true,
        }
    },
    methods:{
        getInfo(){
            axios
            .get(`${API_PROXY}http://apis.juhe.cn/goodbook/query?catalog_id=${this.$route.params.bookId}&pn=1&rn=10&key=9ac34ee4460aaf5ee23762bc7c8554d8`)
            .then(res=>{
                console.log(res);
                let list = res.data.result.data;
                this.bookDetails = this.bookDetails.concat(list);
                if(this.bookDetails.length = 10){
                    this.isLoading = false; 
                }
                // console.log(this.bookDetails);         
            }).catch(()=>{
                alert('出错！');
            });
        },
        handler(){
          this.$router.go(-1);
        }
    },
    created(){
        this.getInfo();
    }
  
}
</script>

<style scoped>
.clearfix::after{
    content: '';
    display: block;
    clear: both;
}
ul{
    margin-top: 1rem;
    margin-bottom:1rem;
    padding: 0 0.1rem;
}
p{
    color: #666;
    font-size: 0.26rem;
    margin-right: 0.8rem;
}
.detail{
    /* display: flex; */
    padding: 0.25rem 0;
    border-bottom: 1px solid #aaa; 
}
.detail-img{
    /* flex-grow: 1; */
    /* width: 0; */
    margin-right: 0.1rem;
    float: left;

}
.detail-img img{
    height: 1.8rem;
    width: 1.28rem;
}
.detail-info{
    /* flex-grow: 2; */
    /* width: 0; */
    float: left;
    position: relative;    
}
.detail-name{
    font-weight: bolder;
    color: #000;
    /* text-align: center; */
    font-size:0.3rem; 
    /* float: left; */
}
.detail-star{
    /* margin-right: 1rem; */
    overflow: hidden;    
    white-space: nowrap;
    text-overflow: ellipsis;
    width: 6rem;
}
.detail-show{
    color:#999;
    margin-right: 0;
    text-overflow: ellipsis;
    display: -webkit-box;
    -webkit-box-orient: vertical;
    -webkit-line-clamp: 8;
    overflow: hidden;
}
span{
    position: absolute;
    right: 0;
    color: #FFB400;
    font-size: .32rem;
    line-height: 1;
}

.loading{
    position: fixed;
    bottom: 1rem;
    text-align: center;
}
</style>
