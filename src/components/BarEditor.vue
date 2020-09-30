<template>
    <div class="editor">
        <div class="input">
            <h3>Data input</h3>

            <div class="data">
                <div class="point" v-for="point in parts" :key="point.index">
                    <form>
                        <input type="text" v-model="point.name" placeholder="Titel" />

                        <textarea v-model="point.desc" placeholder="Beskrivelse" />

                        <input type="number" v-model="point.size" placeholder="Størelse i hele tal" />
                    </form>

                    <button @click="deleteDataPoint(point.index)">Delete data point</button>
                </div>

                <button @click="addDatapoint">Add data point</button>
            </div>
        </div>

        <div class="visual">
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

                            <!--span>{{ part.position }} - {{ part.size }}%</span-->
                        </div>

                        <div class="line" v-if="part.position == 0"></div>
                    </div>
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
                "#ffadad",
                "#ffd6a5",
                "#fdffb6",
                "#caffbf",
                "#9bf6ff",
                "#a0c4ff",
                "#bdb2ff",
                "#ffc6ff",
            ],
            parts: [
                {
                    index: 0,
                    name: "test",
                    desc: "this is a test description",
                    size: 900,
                },
                {
                    index: 1,
                    name: "test",
                    desc: "this is a test description",
                    size: 400,
                },
                {
                    index: 2,
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
                totalSize += parseInt(part.size, 10)
            })

            let partsWithCalculatedPercentage  = []
            let lastPosition = 0
            let lastColorIndex = -1

            //console.log(totalSize)

            this.parts.forEach(part => {
                //console.log(parseInt(part.size, 10))

                let newPart = {
                    name: part.name,
                    desc: part.desc,
                    size: (parseInt(part.size, 10) / totalSize) * 100,
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
    },
    methods: {
        deleteDataPoint(index) {
            this.parts.splice(index, 1)
        },
        addDatapoint() {
            let pointTemplate = {
                index: this.parts.length,
                name: "Data point " + this.parts.length,
                desc: "Edit this description",
                size: 300
            }

            this.parts.push(pointTemplate)
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
    //padding: 300px 0;
    display: grid;
    grid-template-columns: 19% 80%;
}

.visual {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 80vh;
}

.input {
    background:  #D7F3FD;
    padding: 20px;

    h3 {
        //font-weight: 400;
        text-align: center;
        margin-top: 0px
    }

    .point {
        margin-bottom: 10px;
        background: #fff;
        padding: 15px;

        h4 {
            font-weight: 400;
        }

        form {
            input,
            textarea {
                width: 100%;
                margin-bottom: 15px;
                padding: 8px;
                font-family: "Inter";
                outline: 0;
                border: 1px solid lightgrey;
                display: block;
                border-radius: 8px;
            }

            input {
                font-size: 1.2rem;
            }

            textarea {
                font-size: 1rem;
            }
        }
    }
}

.bar {
    margin: 0 auto;
    //max-width: 60vw;
    width: 80%;
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
    min-width: 200px;

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
        font-size: 1.2rem;
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