<script setup>
    import pokemonSearch from "../services/ConnectApi"
    import { ref, onMounted } from "vue";

    const data = ref();
    const searchedValue=ref();

    document.addEventListener("keypress", (event)=>{
        if(event.key=="Enter"){
            event.preventDefault();
        }
        
    })

    function ChangeButton(){
        
        if(document.querySelector("#searchBar input").value.length>0){
            document.querySelector("#searchBar button:nth-of-type(1)").textContent="Find It!" ;                   
        }
        else{
            document.querySelector("#searchBar button:nth-of-type(1)").textContent="Random Pokemon";
        }    
    }

    let emit=defineEmits(['sendDatos']);

    function Search(){
        document.getElementById("divNotFound").classList.add("invisible");
        let searchValue = document.querySelector("#searchBar input").value;
        searchedValue.value=searchValue;
        if(searchValue.length==0){
            searchValue=Math.floor(Math.random()*1010)+1
            
        }else if(isNaN(searchValue))searchValue=searchValue.toLowerCase();    
        
        data.value = pokemonSearch(searchValue);     
        
        (data.value).then( x=> {
            if(x.id!==0){emit('sendDatos', x);}
            else{document.getElementById("divNotFound").classList.remove("invisible");}
        }); 

        document.querySelector("#searchBar input").value="";
        document.querySelector("#searchBar button:nth-of-type(1)").textContent="Random Pokemon";
    }

    onMounted(() => {
        Search();
    })

</script>

<template>
    <section id="searchBar">
        <section id="searchForm">
            <h3>Search your Pokemon by name or number</h3>
            <form>
                <input type="text" placeholder="Find a Pokemon!" autocomplete="on" size="40" @input="ChangeButton()">
                <button type="button" @click="Search()">Random Pokemon</button>
                <button type="reset">Delete</button>
            </form>
        </section>
    </section>
    <div id="divNotFound" class="invisible">
        Couldn't find {{ searchedValue }}
    </div>
</template>

<style scoped>

@import url('https://fonts.googleapis.com/css2?family=Comic+Neue:wght@400;700&display=swap');

#searchBar{border: 1px solid black;
    margin: 0 auto;
    height: fit-content;
    width: 80vw;
    font-family: 'Comic Neue', cursive;
    border-radius: 24px;
    background-color: #3951ED;
}

#searchForm {
    display: flex;
    align-items: center;
    justify-content: space-around;
}

h3{
    color:white;
    font-weight: bold;
    font-size: 1vw;
}

#divNotFound{
    color:#fffc00;
    text-align: center;
}

.invisible{
    display: none;    
}

form{
    width: fit-content;
    display: flex;
    flex-wrap: wrap;
    align-items: center;
    justify-content: center;
    padding: 1vh 1vw;    
    max-width: 100%;
}

input{
    border-radius: 5px;
    padding: 1vh 1vw;
    font-family: 'Comic Neue', cursive; 
    background: #FFD857;
    max-width: 100%;    
}

button{
    width: 13vw;
    background: #ECC139;
    border-radius: 5px;
    margin: auto 0.5vw;
    padding: 1vh 1vw;
    font-family: 'Comic Neue', cursive;
    font-weight: bold;   
    overflow-wrap: break-word; 
    min-width: 15vw;
}

button:hover{
    background: #FFD857;
    transform: scale(1.1);
}

</style>