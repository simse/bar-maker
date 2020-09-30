<template>
    <div class="editor">
        <div class="bar">
            <div
                class="part"
                v-for="part in partsWithPercentage"
                :key="part.name"
                :style="{width: part.size + '%', background: part.color}"
                
            >
                <h2 class="percentage">{{ part.humanReadablePercentage }}%</h2>

                <div :class="{meta: true, above: part.position === 0}" >
                    <div class="line" v-if="part.position == 1"></div>

                    <div class="inner">
                        <h2>{{ part.name }}</h2>
                        <p>{{ part.desc }}</p>

                        <span>{{ part.position }} - {{ part.size }}%</span>
                    </div>

                    <div class="line" v-if="part.position == 0"></div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: "BarEditor",
    data() {
        return {
            colors: [
                "#E1FAF3",
                "#D7FDDD",
                "#D7F3FD",
                "#C1E3D0",
                "#C1E3E3"
            ],
            parts: [
                {
                    name: "test",
                    desc: "this is a test description",
                    size: 900,
                },
                {
                    name: "test",
                    desc: "this is a test description",
                    size: 400,
                },
                {
                    name: "test1",
                    desc: "this is a test description, taking up two thirds",
                    size: 200,
                }
            ]
        }
    },
    computed: {
        partsWithPercentage() {
            let totalSize = 0;
    
            this.parts.forEach(part => {
                totalSize += part.size
            })

            let partsWithCalculatedPercentage  = []
            let lastPosition = 0
            let lastColorIndex = -1

            this.parts.forEach(part => {
                let newPart = {
                    name: part.name,
                    desc: part.desc,
                    size: (part.size / totalSize) * 100,
                    position: (lastPosition === 1 ? 0 : 1),
                    color: this.colors[lastColorIndex + 1],
                    humanReadablePercentage: Math.round((part.size / totalSize) * 1000) / 10
                }

                lastPosition = (lastPosition === 1 ? 0 : 1)
                lastColorIndex = (lastColorIndex > this.colors.length ? -1 : lastColorIndex + 1)

                partsWithCalculatedPercentage.push(newPart)
            })

            return partsWithCalculatedPercentage
        }
    }
}
</script>

<style scoped lang="scss">
$bar-height: 50px;
$popup-spacing: 40px;

.editor {
    //width: 80%;
    //margin: 0 auto;
    padding: 300px 0;
}

.bar {
    margin: 0 auto;
    max-width: 1200px;
    width: 100%;
    //margin: 50px;
    height: $bar-height;
    //background: red;
    display: flex;

    box-shadow:
  0 2.1px 2.2px -8px rgba(0, 0, 0, 0.02),
  0 5.1px 5.3px -8px rgba(0, 0, 0, 0.028),
  0 9.6px 10px -8px rgba(0, 0, 0, 0.035),
  0 17.2px 17.9px -8px rgba(0, 0, 0, 0.042),
  0 32.2px 33.4px -8px rgba(0, 0, 0, 0.05),
  0 77px 80px -8px rgba(0, 0, 0, 0.07)
;



}

.part {
    height: $bar-height;
    //border-right: 1px solid blue;
    position: relative;
    display: flex;
    justify-content: center;
    align-items: center;
    transition: width 300ms ease;

    &:first-child {
        border-top-left-radius: 8px;
        border-bottom-left-radius: 8px;
    }

    &:last-child {
        border-top-right-radius: 8px;
        border-bottom-right-radius: 8px;
    }

    h2.percentage {
        margin: 0;
        font-size: 1.2rem;
        opacity: 0.8;
    }
}

.meta {
    position: absolute;
    top: $bar-height;
    left: 50%;
    // margin-left: -100px;
    transform: translateX(-50%);
    min-width: 300px;

    &.above {
        bottom: $bar-height;
        top: initial;
    }

    .inner {
        background: #D7F3FD;
        padding: 20px;
        border-radius: 6px;
        text-align: center;
    }

    h2 {
        margin: 0;
    }

    p {
        margin-bottom: 0;
    }

    span {
        opacity: 0.5;
        margin-top: 20px;
        display: block;
    }

    .line {
        height: $popup-spacing;
        width: 2px;
        background: #C1E3D0;
        margin: 0 auto;
    }
}
</style>