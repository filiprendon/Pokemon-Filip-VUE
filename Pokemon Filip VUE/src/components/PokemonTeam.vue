<template>
    <div>
        <h2>Your team</h2>
        <div class="pokemon-card" v-for="pokemon in team" :key="pokemon.id">
        </div>
    </div>
</template>

<script>
export default {
    props: ['team']
}
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Press+Start+2P&display=swap');

* {
    font-family: 'Press Start 2P', cursive;
}

p,
h2 {
    text-transform: capitalize;
}

.svg {
    cursor: pointer;
}

.pokemon-card {
    display: flex;
    flex-direction: column;
    margin-bottom: 20px;
    background-color: #f3f3f3;
    border-radius: 10px;
    border: 15px solid #f6f611;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    transition: transform 1.8s ease;
}

.grass {
    background-color: #E9EDC9;
}

.fire {
    background-color: #d19c8e;
}

.water {
    background-color: #a6bed8
}

.poison {
    background-color: #a88ed1;
}

.pokemon-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 20px;
    border-bottom: 1px solid #ccc;
}

.pokemon-name {
    margin: 0;
    font-size: 20px;
}

.pokemon-id {
    margin: 0;
    font-size: 16px;
}

.pokemon-image img {
    width: 100%;
    border-top-left-radius: 10px;
    border-top-right-radius: 10px;
    transition: transform 0.5s ease;
}

.pokemon-image img:hover {
    transform: scale(1.03);
}


.pokemon-details {
    padding: 20px;
}

.pokemon-type {
    margin-bottom: 10px;
    font-size: 14px;
}

.add-to-team-button {
    background-color: #FECA1A;
    color: #3761A8;
    border: none;
    padding: 10px 20px;
    border-radius: 5px;
    cursor: pointer;
    transition: background-color .5s ease;
    width: 100%;
}

.add-to-team-button:hover {
    background-color: #daab11;
    color: #4067ac;
    transition: background-color .5s ease;
}
</style>