<template>
    <div id='container'>
        <div id='firstchild'>
            <div>
                <img src="@/assets/patient.png" alt="medical record">
            </div>
        </div>
        <div id='secondchild'>
            <h1>The medical diary you ever dreamed of</h1><br>
            <h3>We made a tool so you can <br>easily keep track of<br>
            your medicine subscription and appointments.<br>But there is a lot more.</h3>
            <button id='explore' @click='login'>Explore More</button>
        </div>
    </div>
</template>

<script>
export default {
    name: 'Landing',
    methods: {
        login() {
            this.$router.push('/login');
        }
    }
}
</script>

<style>
@import url(http://fonts.googleapis.com/css?family=Noto+Sans:400,700,400italic,700italic&subset=latin,latin-ext);

body {
    background: #22272e;
}

#container {
    width:93%;
    margin-top: 8rem;
    text-align: left;
    justify-content: center;
    align-items: center;
    display: flex;
    font-family: 'Noto Sans';
}

#container #firstchild {
    margin-right: 6rem;
}

img {
    box-shadow: rgba(0, 0, 0, 0.65) 0px 0px 25px;
}

#container #secondchild {
    padding: 2rem 4rem;
    color: #E5C6B3;
}


#explore {
    background-color: #373E47;
    border-radius: 5px;
    font-family: 'Noto Sans';
    padding: 0.5rem;
    margin-top: 1.2rem;
    color:#E5C6B3;
    padding-left: 1.5rem;
    padding-right: 1.5rem;
    border: 1px solid #444c56;
    font-weight: bold;
    cursor: pointer;
}

#explore:hover {
    background-color: #444c56;
}

h1 {
    font-size: 2.8rem;
}

h3 {
    font-size: 0.9rem;
}

</style>