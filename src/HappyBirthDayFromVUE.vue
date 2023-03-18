<template>
    <main>
        <the-congratulations 
            v-if="!showVideo"
            :show-video="showVideo"
            :key="showVideo"
            @change-view="onClick"
        />
        <div v-else class="video">
            <video controls autoplay>
                <source src="./assets/video.mp4" type="video/mp4">
            </video>
            <button @click="onClickVideo">Назад</button>
        </div>
    </main>
</template>

<script>
import TheCongratulations from './components/TheCongratulations.vue';
const MEOWS = ['meow1', 'meow2', 'meow3', 'meow4'];


export default {
    name: 'AndreyHappyBirthday',
    components: {
        TheCongratulations,
    },  
    data() {
        return {
            meows: [],
            showVideo: false,
        }
    },
    mounted() {
        console.warn('Раз ты залез в консоль, придется выслушивать наши поздравления...');
        setTimeout(() => console.error('Упс, не придется, все на экране'), 3000);
        MEOWS.forEach(meow => {
            this.meows.push(this.createMeowAudio(meow));
        });
    },
    methods: {
        createMeowAudio(name) {
            return new Audio(require(`./assets/meows/${name}.mp3`));
        },
        onClickVideo() {
            location.reload();
        },
        onClick() {
            // this.playRandomMeow();
            // this.counter += 1;
            this.showVideo = !this.showVideo;
        },
        playRandomMeow() {
            const idx = Math.floor(Math.random() * this.meows.length);
            const meow = this.meows[idx];
            meow.play();
        },
    },
};
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Montserrat:wght@400;700&family=Roboto:wght@400;700&display=swap');

* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

main {
    background-color: #16161a;
    width: 100vw;
    height: 100vh;
    padding: 50px;

    font-family: 'Lato', sans-serif;
    font-size: 2.5vw;
    color: snow;

    display: flex;
    align-items: center;
    position: relative;
    overflow: hidden;
}

h1 {
    position: relative;
    z-index: 2;
    overflow: hidden;
}

.hi {
    display: inline-block;
    z-index: 1;
}

.box {
    position: absolute;
    bottom: 0;
    width: 18vw;
    height: 10%;
    background-color: #7f5af0;
    z-index: -1;
}

.content {
    display: grid;
    grid-template-columns: max-content 1fr;
    grid-template-areas: 
        'hello animated-text'
        'click-me click-me';
    gap: 16px;
    width: 100%;
}

.hello {
    grid-area: hello;
    height: 7.5vw;
    position: relative;
}

.animated-text {
    grid-area: animated-text;
    height: 100%;
    min-height: 15vw;
}

.click-me {
    grid-area: click-me;
    justify-self: center;
    align-self: center;
}

button {
    all: unset;
    padding: 8px 12px;
    background: #7f5af0;
    font-size: 3vw;
    cursor: pointer;
    transition: all .15s ease-in;
}

button:hover {
    opacity: .8;
}

button:focus {
    opacity: 1;
}

.video {
    display: flex;
    align-items: center;
    justify-content: center;
    width: 100%;
    height: 100%;
}

.video video {
    width: 80%;
    height: 80%;
}

.video button {
    position: absolute;
    z-index: 10;
    top: 0;
    left: 0;
    opacity: 0.25;
}

.congratulations {
    opacity: 0;
    font-size: 30px;
    transition: all .15s ease-in;
}

.hidden {
}

@media (max-width:641px) {
    main {
        padding: 20px;
        
    }

    .video button {
        left: 50%;
        transform: translateX(-50%);
        top: 70px;
        font-size: 3vh;
        opacity: .8;
    }
} 
</style>
