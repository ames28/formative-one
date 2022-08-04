<template>

<div class="aboutContainer">
    <div>
        <h1>About the <i>What's The Story?</i> news app</h1>  
        <h4>Hello, My name is Amy. <br> I am a Web Design Student from New Zealand.</h4> 
        <h4>What's The Story was created as part of a class assignment.</h4>
    </div>
    <div>
        <img src="../assets/webdesign.gif" alt="">
    </div>
    
</div>
 
</template>

<style>

.aboutContainer{
    padding: 20px 0;
    display: flex;
    flex-direction: row;
    justify-content: space-around;
    background-color: antiquewhite;
    color: black;
}

.aboutContainer h4{
    color:black;
}

@media screen and (max-width: 800px) {
    
.aboutContainer{
    flex-direction: column;
    text-align: center;
}

.aboutContainer img{
    width: 50%;
}
}
</style>