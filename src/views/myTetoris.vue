<template>
    <div id="mytetoris">
        <table>
            <tr v-for="i in 20" :key="i">
                <td v-for="j in 10" :key="j" :class="{colored:blockArray[i-1][j-1]}">{{blockArray[i-1][j-1]}}</td>
            </tr>
        </table>
        <button @click="ala([[0,4],[0,5],[1,4],[1,5]])">count</button>
    </div>
</template>

<script>
//var sanctuali = [19,19,19,19,19,19,19,19,19,19];
var sanctuali = [20,20,20,20,20,20,20,20,20,20];
var flag = 0;
export default{
    data(){
        return{
            varone: null,
            blockArray:[]
        }
    },
    methods:{
        ala: function(nowArray){
            //where my block is.
                //flag is for FALLEND
            var flag = 0;
            //Game Over
            for(var i in nowArray){
                if(this.blockArray[nowArray[i][0]][nowArray[i][1]] === 1){
                    alert("Game is over!");
                    //All is reseted
                }
            }
            //var nowArray = [[0,4],[0,5],[0,6],[1,5]];
            //realize my block in the window.
            for(var i=0;i<nowArray.length;i++){
                this.blockArray[nowArray[i][0]].splice(nowArray[i][1],1,1)
            }
            //forbidden area
            var timerId = setInterval(() => {
                //attached sanctuali?
                for(var i=nowArray.length-1;i>=0;i--){
                    if(nowArray[i][0]>=sanctuali[nowArray[i][1]]-2){
                        //get out of here, and please set sanctuali
                        clearInterval(timerId);
                        var flag = 1;
                    }
                }
                //fall myblock
                for(var i=nowArray.length-1;i>=0;i--){
                    this.blockArray[nowArray[i][0]].splice(nowArray[i][1],1,0);
                    this.blockArray[nowArray[i][0]+1].splice(nowArray[i][1],1,1);
                    nowArray[i].splice(0,1,nowArray[i][0]+1);
                }
                //reset sanctuali
                //which line is max at each colmns
                if(flag===1){
                    for (var i=0;i<nowArray.length;i++){
                        if(nowArray[i][0]<sanctuali[nowArray[i][1]]){
                            sanctuali[nowArray[i][1]] = nowArray[i][0];
                        }
                    }
                }
        ///////key command///////////////////////////
            var moves = () => {
            if(event.keyCode == 37){
                //if it isn't at leftside
                var maxleft = 100;
                for(i in nowArray){
                    if(nowArray[i][1]<maxleft){
                        maxleft = nowArray[i][1];
                    }
                }
                console.log(maxleft);
                if(maxleft !== 0){
                    for(i in nowArray){
                        this.blockArray[nowArray[i][0]].splice([nowArray[i][1]],1,0)
                        nowArray[i].splice(1,1,nowArray[i][1]-1)
                    }
                    console.log(sanctuali);
                }
            }
            //right////////////////
            if(event.keyCode == 39){
                //if it isn't at leftside
                var maxright = 0;
                for(i in nowArray){
                    if(nowArray[i][1]>maxright){
                        maxright = nowArray[i][1];
                    }
                }
                console.log(maxright);
                if(maxright < 9){
                    for(i in nowArray){
                        this.blockArray[nowArray[i][0]].splice([nowArray[i][1]],1,0)
                        nowArray[i].splice(1,1,nowArray[i][1]+1)
                    }
                    console.log(sanctuali);
                }
            }
            /////////////////////////
        }
        document.onkeydown = moves ; 
        /////////////////////////////////////////////
            },300)
        }
    },
    mounted(){
        for(var i=0;i<20;i++){
            this.blockArray.push([0,0,0,0,0,0,0,0,0,0]);
            const test = this.blockArray
            test.push([0,0,0,0,0,0,0,0,0,0]);
        }
    },
    watch:{
        blockArray: function(){
            for(var i in blockArray){
                if(blockArray[i].every(value => value == 1){
                    //行のすべてが1だったらすべて削除する。
                }
            }
        }
    }
}
</script>
<style>
table {
    width:auto;
    height:100%;
    margin:auto;
    border:1px solid black;
}
td {
    height:25px;
    width:25px;
    background-color: antiquewhite;
    border:1px solid blue;
}
.colored{
    background-color: red;
}
</style>