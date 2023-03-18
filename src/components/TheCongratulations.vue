<template>
    <h1
        v-if="!showVideo"
        class="content"
    >
        <div class="hello">
            <span class="box"></span>
            <span class="hi">Андрей</span>
        </div>
        <div class="animated-text">
            <span class="text"></span>
            <span class="cursor">_</span>
        </div>
        <div class="click-me">
            <button @click="onClick">Кликни</button>
        </div>
    </h1>
</template>

<script>
import gsap from 'gsap';
import TextPlugin from 'gsap/TextPlugin';
import EasePack from 'gsap/EasePack';
gsap.registerPlugin(TextPlugin, EasePack.RoughEase);

const consoleWords = [
    'С новым счастьем',
    'Чтобы твои желания офигевали от твоих возможностей!',
    // 'Еще на год ближе к смерти',
    'Ты лучший!',
    'Ты потрясающий!',
    'Лопни от счастья',
    'Ты офигенный!',
    'С тобой приятно работать',
    'Ты надежный',
    'Счастливого чего там у тебя',
    'Мы в тебя верим и ценим тебя',
    'Ты супер-лид!',
    'Ты крутой профессионал',
    'привееет Андреееей',
    'привет Андреееееееей',
    'привееееееет Андрееееееееееееей',
    'ну где ж ты был',
    'ну обними нас скорей!',
    'Чтоб у тебя все было и тебе за это ничего не было',
    'Море удачи и дачу у моря',
    'Бодрости духа и ясности ума'
];

const mainWords = [
    'ты классный разработчик',
    'ты отличный техлид',
    'ты приятный человек:)',
    'мы рады с тобой работать',
    'c новым годом жизни!',
    'мы тебя обняли, приподняли, покружили, поставили',
];

let masterTl;

export default {
    props: {
        showVideo: {
            required: true,
            type: Boolean,
            default: false,
        }
    },
    data() {
        return {
            words: mainWords,
            counter: 0,
        }
    },
    mounted() {
        setInterval(this.showCongratulationsInRandomPlace, 1500);
        // Animations
        this.animateCursor();
        this.glowBox();
        this.mainAnimation();
    },
    methods: {
        logCongratulations() {
            if (this.counter === consoleWords.length) {
                this.counter = 0;
            }

            console.log(consoleWords[this.counter]);
            this.counter += 1;
        },
        animateCursor() {
            gsap.to('.cursor', { opacity: 0, repeat: -1, ease: 'power1.out' });
        },
        glowBox() {
            gsap.to('.box', {
                opacity: 0.8,
                ease: 'rough({strength: 3, points: 25, template: Power1.easeOut, taper: both, randomize: false, clamp: true})',
                yoyo: true,
                repeat: -1,
                duration: 2,
            });
        },
        mainAnimation() {
            masterTl = gsap.timeline({ paused: true, repeat: 0, repeatDelay: 1 });

            const box = document.querySelector('.box');
            const hi = document.querySelector('.hi');
            const text = document.querySelector('.text');
            const clickMe = document.querySelector('.click-me');

            const boxTl = gsap.timeline();
            boxTl
                .from(box, { width: 0, ease: 'power1.out', delay: 1 })
                .from(hi, { y: '100%', opacity: '0' })
                .to(box, { height: '100%', ease: 'elastic.out(1, 0.4)', delay: 0.5 });

            masterTl.add(boxTl);

            this.words.forEach(word => {
                let wordTl = gsap.timeline({ delay: 0.5, yoyo: true, repeat: 1, repeatDelay: 1 });
                wordTl.to(text, { text: word, duration: 1 });
                masterTl.add(wordTl);
            });

            const finalWordsTl = gsap.timeline({ delay: 0.5, yoyo: false });
            finalWordsTl.to(text, { text: 'ты должен кликнуть.', duration: 1 });
            masterTl.add(finalWordsTl);

            const showButtonTl = gsap.timeline();
            showButtonTl.to(box, { backgroundColor: '#16161a' });
            showButtonTl.from(clickMe, { opacity: 0, backgroundColor: '#000' });

            masterTl.add(showButtonTl);

            masterTl.play();
        },
        onClick() {
            // this.playRandomMeow();
            // this.counter += 1;
            masterTl.kill();
            this.$emit('change-view');
        },
        showCongratulationsInRandomPlace() {
            if (this.showVideo) {
                return;
            }

            if (this.counter === consoleWords.length) {
                this.counter = 0;
            }

            var width = window.innerWidth / 2;
            var heighth = window.innerHeight / 2;

            var elem = document.createElement("div");
            var main = document.body.querySelector('main');
            elem.textContent = consoleWords[this.counter];

            elem.style.position = "absolute";
            elem.classList.add('congratulations');
            elem.style.left = Math.round(Math.random() * width) + "px";
            elem.style.top = Math.round(Math.random() * heighth) + "px";
            main.appendChild(elem);
            gsap.to(elem, { opacity: .1, yoyo: true })
            
            setTimeout(() => {
                gsap.to(elem, { opacity: 0 })
            }, 1200);

            this.counter += 1;
        }
    }
    
}
</script>

<style scoped>
@media (max-width:641px) {
    .content {
        display: flex;
        flex-direction: column;
        gap: 30px;
    }

    h1 {
        font-size: 5vh;
    }

    .box {
        width: 50vw;
        height: 10vh;
        background-color: #7f5af0;
        z-index: -1;
    }

    .hello {
        height: 5vh;
        display: flex;
        align-items: center;
        justify-content: center;
        position: relative;
    }

    .animated-text {
        display: flex;
        align-items: center;
        justify-content: center;
        min-height: 20vh;
    }
    .animated-text .text {
        display: flex;
        align-items: center;
        justify-content: center;
        text-align: center;
    }

    .click-me button {
        font-size: 3vh;
    }
} 
</style>