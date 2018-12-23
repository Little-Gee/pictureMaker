<template>
<div class="container">
    <div class="preview">
        <canvas
            ref="myCanvas"
            width="500"
            height="300">
        </canvas>
    </div>
    <div>
        文字：
        <el-input v-model="input" placeholder="请输入内容"></el-input>
    </div>
    <div class="btn-group">
        <el-button type="primary" @click="changeFontSize('bigger')">字体变大</el-button>
        <el-button type="primary" @click="changeFontSize('smaller')">字体变小</el-button>
        <el-button type="primary" @click="changeBgColor">换背景颜色</el-button>
        <el-button type="primary" @click="changeTextColor">换文字颜色</el-button>
        <el-button type="primary" @click="convertCanvasToImage">保存</el-button>
    </div>
</div>
</template>

<script>
export default {
    data() {
        return {
            input: '',
            fontSize: 50,
            bgColor: '',
            textColor: ''
        }
    },
    watch: {
        input() {
            this.create();
        }
    },
    mounted() {
        this.bgColor = this.randomColor();
        this.textColor = this.randomColor();
        this.create();
    },
    methods: {
        create() {
            const canvas = this.$refs.myCanvas;
            let canvasCxt = canvas.getContext('2d');
            canvasCxt.fillStyle = this.bgColor;
            canvasCxt.fillRect(0, 0, 500, 300);
            this.canvasText(canvasCxt, this.input, `${this.fontSize}px bolder 黑体`, this.textColor);
        },
        changeBgColor() {
            this.bgColor = this.randomColor();
            this.create();
        },
        changeTextColor() {
            this.textColor = this.randomColor();
            this.create();
        },
        // 随机生成颜色
        randomColor() {
            let color="#";
			for(let i=0;i<6;i++){
				color += (Math.random()*16 | 0).toString(16);
			}
			return color;
        },
        changeFontSize(type) {
            if (this.fontSize == 0) {
                this.fontSize = 1;
            }
            if (type == 'bigger') {
                this.fontSize++;
            } else {
                this.fontSize--;
            }
            this.create();
        },
        canvasText(paint, text, fontSize, color) {
            const canvas = this.$refs.myCanvas;
            paint.font = fontSize;
            paint.fillStyle = color;
            paint.textAlign = "center";
            paint.fillText(text, canvas.width / 2, canvas.height / 1.9);
        },
        convertCanvasToImage() {
            const canvas = this.$refs.myCanvas;
            let image = new Image();
            image.src = canvas.toDataURL("image/png");
            let a = document.createElement("a");
            a.href = image.src;
            a.download = `${this.input}.png`;
            a.click();
        }
    }
}
</script>

<style lang="less" scoped>
.container {
    display: flex;
    flex-direction: column;
    justify-content: center;
    padding: 30px;

    .preview {
        display: flex;
        justify-content: center;
        margin-bottom: 30px;
    }
    .el-input {
        width: 300px;
    }
    .btn-group {
        text-align: end;
        margin-top: 20px;
    }
}
</style>