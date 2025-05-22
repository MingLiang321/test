<template>
    <el-container class="demo w100 h100">
        <el-main height="calc(100%)">
            <div id="canvasDiv" class="w100 h100">
                <canvas ref="streetCanvas" :width="canvasWidth" :height="canvasHeight" @mousedown="startDrag"
                    @mousemove="drag" @mouseup="stopDrag" @mouseleave="stopDrag"></canvas>

                <div v-for="(div, index) in divs" :key="index" class="overlayDiv"
                    :style="{ left: `${div.left}%`, top: `${div.top}%`, transform: `translate(${offsetX}px, ${offsetY}px)` }">
                    <div class="box">
                        <img :src="div.img"
                            :style="{ width: `${div.imgWidth}vw`, height: `${div.imgheight}vw`, position: 'absolute', 'z-index': '2', left: `${div.imgLeft}%`, top: `${div.imgTop}%` }"
                            @click="openDialog()">
                        <img :src="div.img2" style="width: 6.57vw;height: 4.5vw;position: absolute;"
                            @click="openDialog()">
                        <div class="particles" style="width: 6.57vw;height: 4.5vw;">
                            <span></span>
                            <span></span>
                            <span></span>
                            <span></span>
                            <span></span>
                            <span></span>
                            <span></span>
                            <span></span>
                            <span></span>
                            <span></span>
                            <span></span>
                            <span></span>
                            <span></span>
                        </div>
                    </div>
                    <border height="" class="info" :style="{ left: `${div.left2}%`, top: `${div.top2}%` }">
                        <el-row>
                            <el-col :span="24" class="colTitle">MK-7-020</el-col>
                        </el-row>
                        <el-row>
                            <el-col :span="12">焊接电压</el-col>
                            <el-col :span="12" class="colCss">220V</el-col>
                        </el-row>
                        <el-row>
                            <el-col :span="12">焊接电流</el-col>
                            <el-col :span="12" class="colCss">20A</el-col>
                        </el-row>
                        <el-row>
                            <el-col :span="12">基准电压</el-col>
                            <el-col :span="12" class="colCss">200-220V</el-col>
                        </el-row>
                        <el-row>
                            <el-col :span="12">基准电流</el-col>
                            <el-col :span="12" class="colCss">10A-20A</el-col>
                        </el-row>
                        <el-row>
                            <el-col :span="12">送线速度</el-col>
                            <el-col :span="12" class="colCss">10A-20A</el-col>
                        </el-row>
                        <el-row>
                            <el-col :span="12">JOB号</el-col>
                            <el-col :span="12" class="colCss">/</el-col>
                        </el-row>
                        <el-row>
                            <el-col :span="12">热输入</el-col>
                            <el-col :span="12" class="colCss">22</el-col>
                        </el-row>
                    </border>
                </div>
            </div>
        </el-main>

        <div @click="handleOpen" class="fold">
            <el-icon v-show="!drawerVisible">
                <Fold />
            </el-icon>
            <el-icon v-show="drawerVisible">
                <Expand />
            </el-icon>
        </div>

        <div class=".bottomDrawer">
            <el-drawer v-model="drawerVisible" :direction="'btt'" :show-close="false" :size="'11%'">
                <div style="display: inline-block;width: 50%;">
                    <div
                        style="color: dodgerblue;display: flex;justify-content: space-between;text-align: center;font-size: 1vw;">
                        <div>
                            <div>設備數量</div>
                            <div style="margin-top: 0.5vw;">
                                <div style="position: relative;">
                                    <div style="position: absolute;width: 100%;">21</div>
                                </div>
                                <img src="../assets/imgs/呈现.png" style="width: 50%;">
                            </div>
                        </div>
                        <div style="color: aqua;">
                            <div>正常工作</div>
                            <div style="margin-top: 0.5vw;">
                                <div style="position: relative;">
                                    <div style="position: absolute;width: 100%;">18</div>
                                </div>
                                <img src="../assets/imgs/呈现.png" style="width: 50%;">
                            </div>
                        </div>
                        <div style="color: aqua;">
                            <div>待機</div>
                            <div style="margin-top: 0.5vw;">
                                <div style="position: relative;">
                                    <div style="position: absolute;width: 100%;">1</div>
                                </div>
                                <img src="../assets/imgs/呈现.png" style="width: 50%;">
                            </div>
                        </div>
                        <div style="color: #ffffff;">
                            <div>停機</div>
                            <div style="margin-top: 0.5vw;">
                                <div style="position: relative;">
                                    <div style="position: absolute;width: 100%;">1</div>
                                </div>
                                <img src="../assets/imgs/呈现.png" style="width: 50%;">
                            </div>
                        </div>
                        <div style="color: red;">
                            <div>故障</div>
                            <div style="margin-top: 0.5vw;">
                                <div style="position: relative;">
                                    <div style="position: absolute;width: 100%;">1</div>
                                </div>
                                <img src="../assets/imgs/呈现.png" style="width: 50%;">
                            </div>
                        </div>
                    </div>
                </div>

            </el-drawer>
        </div>

        <div class=".leftDrawer">
            <el-drawer v-model="drawerVisible" :direction="'ltr'" :show-close="false" :size="'25%'">
                <div
                    style="background: url('src/assets/imgs/边框左.png') no-repeat;background-size: 100% 100%;height: 33.3%;padding: 1vw;">
                    <div style="font-size: 0.9vw;font-weight: bold;color: aquamarine;">
                        <img src="../assets/imgs/球.png" style="width: 10%;vertical-align: middle;">
                        <span>总览</span>
                    </div>
                    <table class="h80 tc" style="font-size: 0.9vw;">
                        <tbody>
                            <tr>
                                <td>
                                    <img src="../assets/imgs/dy.png" style="width: 10%;vertical-align: middle;">&nbsp;
                                    <span>焊接电压</span>
                                </td>
                                <td style="color: aqua;font-size: 1vw;font-weight: bold;">
                                    200V
                                </td>
                            </tr>
                            <tr>
                                <td><img src="../assets/imgs/dl.png" style="width: 10%;vertical-align: middle;">&nbsp;
                                    <span>焊接电流</span>
                                </td>
                                <td style="color: aqua;font-size: 1vw;font-weight: bold;">
                                    20A
                                </td>
                            </tr>
                            <tr>
                                <td>
                                    <img src="../assets/imgs/sxsd.png" style="width: 10%;vertical-align: middle;">&nbsp;
                                    <span>送线速度</span>
                                </td>
                                <td style="color: aqua;font-size: 1vw;font-weight: bold;">
                                    20A
                                </td>
                            </tr>
                            <tr>
                                <td>
                                    <img src="../assets/imgs/qt.png" style="width: 10%;vertical-align: middle;">&nbsp;
                                    <span>热输入</span>
                                </td>
                                <td style="color: aqua;font-size: 1vw;font-weight: bold;">
                                    30
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </div>
                <div
                    style="background: url('src/assets/imgs/边框左.png') no-repeat;background-size: 100% 100%;height: 33.3%;padding: 1vw;">
                    <div style="font-size: 0.9vw;font-weight: bold;color: aquamarine;">
                        <img src="../assets/imgs/球.png" style="width: 10%;vertical-align: middle;">
                        <span>用能趋势</span>
                    </div>
                    <div id="energyBar" class="w100 h80"></div>
                </div>
                <div
                    style="background: url('src/assets/imgs/边框左.png') no-repeat;background-size: 100% 100%;height: 33.3%;padding: 1vw;">
                    <div style="font-size: 0.9vw;font-weight: bold;color: aquamarine;">
                        <img src="../assets/imgs/球.png" style="width: 10%;vertical-align: middle;">
                        <span>热输入趋势</span>
                    </div>
                    <div id="heatInputLine" class="w100 h80"></div>
                </div>
            </el-drawer>
        </div>

        <div class=".rightDrawer">
            <el-drawer v-model="drawerVisible" :direction="'rtl'" :show-close="false" :size="'25%'">
                <div
                    style="background: url('src/assets/imgs/边框右.png') no-repeat;background-size: 100% 100%;height: 33.3%;padding: 1vw;">
                    <div style="font-size: 0.9vw;font-weight: bold;color: aquamarine;">
                        <img src="../assets/imgs/球.png" style="width: 10%;vertical-align: middle;">
                        <span>设备利用率</span>
                    </div>
                    <div id="efficiencyLine" class="w100 h80"></div>
                </div>
                <div
                    style="background: url('src/assets/imgs/边框右.png') no-repeat;background-size: 100% 100%;height: 33.3%;padding: 1vw;">
                    <div style="font-size: 0.9vw;font-weight: bold;color: aquamarine;">
                        <img src="../assets/imgs/球.png" style="width: 10%;vertical-align: middle;">
                        <span>UPH</span>
                    </div>
                    <div id="UPHBar" class="w100 h80"></div>
                </div>
                <div
                    style="background: url('src/assets/imgs/边框右.png') no-repeat;background-size: 100% 100%;height: 33.3%;padding: 1vw;">
                    <div style="font-size: 0.9vw;font-weight: bold;color: aquamarine;">
                        <img src="../assets/imgs/球.png" style="width: 10%;vertical-align: middle;">
                        <span>实时报警</span>
                    </div>
                    <el-table height="80%" :data="componentsList" border="1" style="width: 99%;font-size: 0.7vw;"
                        highlight-current-row>
                        <el-table-column prop="name" label="报警对象" align="center" />
                        <el-table-column prop="spec" label="报警时间" align="center" />
                        <el-table-column prop="qty" label="报警描述" align="center" />
                    </el-table>
                </div>
            </el-drawer>
        </div>

        <el-dialog v-model="dialogVisible" :modal="false">
            <div style="color: #ffffff;height: 100%;font-size: 1vw;">
                <div style="border-bottom: 2px solid skyblue;padding-bottom: 10px;margin: 0 5px;height: 10%;">
                    <div style="display: inline-block;width: 50%;">
                        設備視覺化
                    </div>
                    <div style="display: inline-block;width: 50%;">
                        {{ currTime }}
                    </div>
                </div>
                <div style="padding: 20px 0;margin: 0 5px;height: 10%;">
                    <div style="display: inline-block;width: 50%;">
                        <div style="display: inline-block;">今日產量&nbsp;10&nbsp;件</div>
                        <div style="display: inline-block;margin-left: 2%;">本週產量&nbsp;10&nbsp;件</div>
                        <div style="display: inline-block;margin-left: 2%;">本月產量&nbsp;10&nbsp;件</div>
                        <div style="display: inline-block;margin-left: 2%;">年度產量&nbsp;10&nbsp;件</div>
                    </div>
                    <div style="display: inline-block;width: 50%;text-align: right;">
                        <el-button type="primary" @click="search()">警報訊息</el-button>
                        <el-button type="primary" @click="search()">數據統計</el-button>
                    </div>
                </div>
                <el-row style="height:25vw;">
                    <el-col :span="10">
                        <div
                            style="background: url('src/assets/imgs/bk2.png') no-repeat;background-size: 100% 100%;height: 100%;width: 100%;">
                            <div style="padding: 5% 0 0 5%;">機械手臂</div>
                            <div style="padding: 2% 0 0 5%;">目前設備運作狀態</div>
                            <div style="text-align: center;margin: 10% 0;">
                                <img src="../assets/imgs/机械臂.png" style="width: 50%;">
                                <div style="position: relative;">
                                    <div class="info" style="top: -10vw;left: 2vw;font-size: 0.6vw;">
                                        <table style="text-align: center;width: 100%;color: #F5F5DC;">
                                            <tbody>
                                                <tr>
                                                    <td colspan="2" style="font-weight: bold;color: #ffffff;">
                                                        MK-7-020
                                                    </td>
                                                </tr>
                                                <tr>
                                                    <td>焊接电压</td>
                                                    <td style="color: dodgerblue;">220V</td>
                                                </tr>
                                                <tr>
                                                    <td>焊接电流</td>
                                                    <td>20A</td>
                                                </tr>
                                                <tr>
                                                    <td>基准电压</td>
                                                    <td>200-220V</td>
                                                </tr>
                                                <tr>
                                                    <td>基准电流</td>
                                                    <td>10A-20A</td>
                                                </tr>
                                                <tr>
                                                    <td>送线速度</td>
                                                    <td>10A-20A</td>
                                                </tr>
                                                <tr>
                                                    <td>JOB号</td>
                                                    <td>/</td>
                                                </tr>
                                                <tr>
                                                    <td>型号</td>
                                                    <td>22</td>
                                                </tr>
                                                <tr>
                                                    <td>热输入</td>
                                                    <td>22</td>
                                                </tr>
                                            </tbody>
                                        </table>
                                    </div>
                                </div>
                            </div>
                        </div>

                    </el-col>
                    <el-col :span="7">
                        <div
                            style="background: url('src/assets/imgs/bk.png') no-repeat;background-size: 100% 100%;height: 49.5%;">
                            <div id="StateGantt" class="w100 h100"></div>
                        </div>
                        <div
                            style="background: url('src/assets/imgs/bk.png') no-repeat;background-size: 100% 100%;height: 49.5%;margin-top: 1%;">
                            <div id="UPHBar2" class="w100 h100"></div>
                        </div>
                    </el-col>
                    <el-col :span="7">
                        <div
                            style="background: url('src/assets/imgs/bk.png') no-repeat;background-size: 100% 100%;height: 49.5%;">
                            <div id="efficiencyLine2" class="w100 h100"></div>
                        </div>
                        <div
                            style="background: url('src/assets/imgs/bk.png') no-repeat;background-size: 100% 100%;height: 49.5%;margin-top: 1%;">
                            <div id="MTBar" class="w100 h100"></div>
                        </div>
                    </el-col>
                </el-row>
                <div style="height: 13vw;">
                    <el-table height="100%" :data="componentsList" :border="true"
                        style="width: 99%;margin: 1% 0 0 5px;font-size: 0.7vw;" highlight-current-row>
                        <el-table-column prop="name" label="报警描述" align="center" />
                        <el-table-column prop="spec" label="报警时间" align="center" />
                        <el-table-column prop="qty" label="报警值" align="center" />
                        <el-table-column prop="a" label="报警限值" align="center" />
                        <el-table-column prop="b" label="报警等级" align="center" />
                    </el-table>
                </div>

            </div>
        </el-dialog>

    </el-container>
</template>

<script setup>
import service from "../utils/request.js";
import * as echarts from "echarts";
import { ref } from "vue";

const componentsList = ref([{ "name": "支架贴片-左", "spec": "T=1.0mm HC600/900QP", "qty": 1, "a": 1, "b": 1 },
{ "name": "支架贴片-左", "spec": "T=1.0mm HC600/900QP", "qty": 1, "a": 1, "b": 1 }, { "name": "支架贴片-左", "spec": "T=1.0mm HC600/900QP", "qty": 1, "a": 1, "b": 1 }, { "name": "支架贴片-左", "spec": "T=1.0mm HC600/900QP", "qty": 1, "a": 1, "b": 1 }, { "name": "支架贴片-左", "spec": "T=1.0mm HC600/900QP", "qty": 1, "a": 1, "b": 1 }])
const rawData = ref([]);
const dept = ref(null);
const category = ref(["1:00", "2:00", "3:00", "4:00", "5:00", "6:00", "7:00"]);
const humiArr = ref([1, 9, 5, 8, 2, 6, 0]);
const humiArr2 = ref([3, 6, 7, 8, 8, 1, 0]);
let lineChartInstance1 = null;
let lineChartInstance2 = null;
let lineChartInstance3 = null;
let barChartInstance1 = null;
let barChartInstance2 = null;
let barChartInstance3 = null;
let barChartInstance4 = null;
let ganttChartInstance = null;
const dialogVisible = ref(false);
const drawerVisible = ref(true);
const currTime = ref('');
const streetCanvas = ref(null);
const canvasWidth = ref(0);
const canvasHeight = ref(0);
const scale = ref(1);
const divs = reactive([
    { left: 26, top: 19, left2: -75, top2: -150, img: 'src/assets/imgs/东南朝向.png', imgWidth: 4.4, imgheight: 4, imgLeft: 35, imgTop: -10, img2: 'src/assets/imgs/運行狀態框.png' },
    { left: 18.5, top: 28, left2: -75, top2: -150, img: 'src/assets/imgs/东南朝向.png', imgWidth: 4.4, imgheight: 4, imgLeft: 35, imgTop: -10, img2: 'src/assets/imgs/運行狀態框.png' },
    { left: 11, top: 37, left2: -75, top2: -150, img: 'src/assets/imgs/东南朝向.png', imgWidth: 4.4, imgheight: 4, imgLeft: 35, imgTop: -10, img2: 'src/assets/imgs/運行狀態框.png' },

    { left: 44, top: 16, left2: 100, top2: -160, img: 'src/assets/imgs/西南.png', imgWidth: 4.288, imgheight: 4, imgLeft: 25, imgTop: 0, img2: 'src/assets/imgs/運行狀態框.png' },
    { left: 51, top: 24, left2: 100, top2: -160, img: 'src/assets/imgs/西南.png', imgWidth: 4.288, imgheight: 4, imgLeft: 25, imgTop: 0, img2: 'src/assets/imgs/運行狀態框.png' },
    { left: 58, top: 32, left2: 100, top2: -160, img: 'src/assets/imgs/西南.png', imgWidth: 4.288, imgheight: 4, imgLeft: 25, imgTop: 0, img2: 'src/assets/imgs/運行狀態框.png' },
    { left: 65, top: 40, left2: 100, top2: -160, img: 'src/assets/imgs/西南.png', imgWidth: 4.288, imgheight: 4, imgLeft: 25, imgTop: 0, img2: 'src/assets/imgs/運行狀態框.png' },
    { left: 72, top: 48, left2: 100, top2: -160, img: 'src/assets/imgs/西南.png', imgWidth: 4.288, imgheight: 4, imgLeft: 25, imgTop: 0, img2: 'src/assets/imgs/運行狀態框.png' },

    { left: 38, top: 23, left2: -100, top2: 60, img: 'src/assets/imgs/關機狀態東北.png', imgWidth: 4.638, imgheight: 4, imgLeft: 30, imgTop: -10, img2: 'src/assets/imgs/關機狀態框.png' },
    { left: 45, top: 31, left2: -100, top2: 60, img: 'src/assets/imgs/東北.png', imgWidth: 4.638, imgheight: 4, imgLeft: 30, imgTop: -10, img2: 'src/assets/imgs/運行狀態框.png' },
    { left: 52, top: 39, left2: -100, top2: 60, img: 'src/assets/imgs/東北.png', imgWidth: 4.638, imgheight: 4, imgLeft: 30, imgTop: -10, img2: 'src/assets/imgs/運行狀態框.png' },
    { left: 59, top: 47, left2: -100, top2: 60, img: 'src/assets/imgs/東北.png', imgWidth: 4.638, imgheight: 4, imgLeft: 30, imgTop: -10, img2: 'src/assets/imgs/異常、維修框.png' },
    { left: 66, top: 55, left2: -100, top2: 60, img: 'src/assets/imgs/東北.png', imgWidth: 4.638, imgheight: 4, imgLeft: 30, imgTop: -10, img2: 'src/assets/imgs/運行狀態框.png' },
    { left: 73, top: 63, left2: -100, top2: 60, img: 'src/assets/imgs/東北.png', imgWidth: 4.638, imgheight: 4, imgLeft: 30, imgTop: -10, img2: 'src/assets/imgs/運行狀態框.png' },
    { left: 80, top: 71, left2: -100, top2: 60, img: 'src/assets/imgs/東北.png', imgWidth: 4.638, imgheight: 4, imgLeft: 30, imgTop: -10, img2: 'src/assets/imgs/運行狀態框.png' },

    { left: 23, top: 43, left2: -100, top2: 60, img: 'src/assets/imgs/西南.png', imgWidth: 4.288, imgheight: 4, imgLeft: 25, imgTop: 0, img2: 'src/assets/imgs/運行狀態框.png' },
    { left: 30, top: 52, left2: -100, top2: 60, img: 'src/assets/imgs/西南.png', imgWidth: 4.288, imgheight: 4, imgLeft: 25, imgTop: 0, img2: 'src/assets/imgs/運行狀態框.png' },
    { left: 37, top: 61, left2: -100, top2: 60, img: 'src/assets/imgs/西南.png', imgWidth: 4.288, imgheight: 4, imgLeft: 25, imgTop: 0, img2: 'src/assets/imgs/運行狀態框.png' },
    { left: 44, top: 70, left2: -100, top2: 60, img: 'src/assets/imgs/西南.png', imgWidth: 4.288, imgheight: 4, imgLeft: 25, imgTop: 0, img2: 'src/assets/imgs/運行狀態框.png' },
    { left: 51, top: 79, left2: -100, top2: 60, img: 'src/assets/imgs/東北2.png', imgWidth: 5.333, imgheight: 4, imgLeft: 20, imgTop: 0, img2: 'src/assets/imgs/運行狀態框.png' },
    { left: 58, top: 88, left2: -100, top2: 60, img: 'src/assets/imgs/東北2.png', imgWidth: 5.333, imgheight: 4, imgLeft: 20, imgTop: 0, img2: 'src/assets/imgs/運行狀態框.png' },
]);

onMounted(() => {
    setInterval(getCurrTime, 1000);
    search();
    showUPHBarChart(["08:00", "09:00", "10:00", "11:00", "12:00", "13:00", "14:00", "15:00", "16:00", "17:00", "18:00", "18:00"], [5, 20, 36, 10, 10, 20]);
    showEfficiencyLineChart(category.value, humiArr.value, humiArr2.value);
    showHeatInputLineChart(category.value, [1, 3, 4, 8, 9]);
    showEnergyBarChart();

    setCanvasSize();
    setTimeout(() => {
        render();
        updateScale();
    }, 100);

});

// 平移偏移量
const offsetX = ref(0);
const offsetY = ref(0);

// 拖动相关变量
let isDragging = false;
let lastMouseX = 0;
let lastMouseY = 0;

// 街道参数
const streetWidth = 30; // 街道宽度
const vLineLength = 1600; // 纵线长度
const hLineLength = 1100; // 横线长度

// 初始化画布尺寸
const setCanvasSize = () => {
    var canvasDiv = document.getElementById("canvasDiv");
    canvasWidth.value = canvasDiv.offsetWidth;
    canvasHeight.value = canvasDiv.offsetHeight;

    //1900
    //899
    console.log(canvasWidth.value);
    console.log(canvasHeight.value);
    console.log(canvasWidth.value / canvasHeight.value);
};

// 绘制街道
const drawStreet = (ctx, centerX, centerY, width, height, angle, color, shadowColor) => {
    ctx.save(); // 保存画布状态

    // 移动到旋转中心
    ctx.translate(centerX, centerY);
    ctx.rotate(angle); // 旋转街道

    // 绘制街道本体
    ctx.fillStyle = color;
    ctx.fillRect(-width / 2, -height / 2, width, height);

    ctx.restore(); // 恢复画布状态
};

// 渲染函数
const render = () => {

    const canvas = streetCanvas.value;
    const ctx = canvas.getContext("2d");

    // 清空画布
    ctx.clearRect(0, 0, canvasWidth.value, canvasHeight.value);
    ctx.setTransform(1, 0, 0, 1, 0, 0); // 重置变换

    // 平移并旋转画布
    ctx.save();
    ctx.translate(offsetX.value, offsetY.value);

    // 绘制街道1
    drawStreet(
        ctx,
        canvasWidth.value / 2 - 425, // 中心点X
        canvasHeight.value / 2 - 125, // 中心点Y
        vLineLength,
        streetWidth,
        (150 * Math.PI) / 180,
        "#1e3f70", // 街道颜色
        "#404040" // 阴影颜色
    );

    // 纵向街道2
    drawStreet(
        ctx,
        canvasWidth.value / 2 + 500, // 中心点X
        canvasHeight.value / 2 + 455, // 中心点Y
        vLineLength,
        streetWidth,
        (145 * Math.PI) / 180,
        "#1e3f70", // 街道颜色
        "#404040" // 阴影颜色
    );

    // 横向街道
    drawStreet(
        ctx,
        canvasWidth.value / 2 + 475, // 中心点X
        canvasHeight.value / 2 - 125, // 中心点Y
        hLineLength,
        streetWidth,
        (26 * Math.PI) / 180,
        "#1e3f70", // 街道颜色
        "#404040" // 阴影颜色
    );
    drawStreet(
        ctx,
        canvasWidth.value / 2 + 100, // 中心点X
        canvasHeight.value / 2 + 125, // 中心点Y
        hLineLength,
        streetWidth,
        (30 * Math.PI) / 180,
        "#1e3f70", // 街道颜色
        "#404040" // 阴影颜色
    );
    drawStreet(
        ctx,
        canvasWidth.value / 2 - 260, // 中心点X
        canvasHeight.value / 2 + 355, // 中心点Y
        hLineLength,
        streetWidth,
        (33 * Math.PI) / 180,
        "#1e3f70", // 街道颜色
        "#404040" // 阴影颜色
    );

    // 恢复画布状态
    ctx.restore();

    // 监听窗口大小变化
    window.addEventListener("resize", handleResize);
};

// 鼠标事件
const startDrag = (e) => {
    isDragging = true;
    lastMouseX = e.clientX;
    lastMouseY = e.clientY;
};

const drag = (e) => {
    if (isDragging) {
        const dx = e.clientX - lastMouseX;
        const dy = e.clientY - lastMouseY;

        offsetX.value += dx;
        offsetY.value += dy;

        lastMouseX = e.clientX;
        lastMouseY = e.clientY;

        render(); // 重新渲染
    }
};

const stopDrag = () => {
    isDragging = false;
};

const handleOpen = () => {
    if (drawerVisible.value) {
        drawerVisible.value = false;
    } else {
        drawerVisible.value = true;
    }

}

const search = () => {
    rawData.value = [];
    service.get("http://10.9.0.12:1880/GetConfigForTH", {
        params: {
            WA: "BP",
            Dept: dept.value
        }
    }).then(res => {
        //console.log("res.DATA", res.DATA);
        let tempData = [];
        if (res.length > 0) {
            tempData = res[0].DATA;
        }
        else {
            return;
        }
        //console.log("过滤后的数据:", tempData);
        tempData.forEach(item => {
            let param = item.Location.MID;
            //console.log("param",param)
            service.get("http://10.9.0.12:1880/GetTHRealtime?Location=" + param).then(res => {
                if (res && res.Location) {
                    rawData.value.push(res);
                }
            }).catch(error => {
                //console.error(param + "数据获取失败:", error);
            });
        });
        //console.log("详细数据:", rawData.value);
    }).catch(error => {
        //console.error("数据获取失败:", error);
    });
};

const openDialog = () => {
    dialogVisible.value = true;
    showEfficiencyLineChart2(category.value, humiArr.value, humiArr2.value);
    showUPHBarChart2(["08:00", "09:00", "10:00", "11:00", "12:00", "13:00", "14:00", "15:00", "16:00", "17:00", "18:00", "18:00"], [5, 20, 36, 10, 10, 20, 50, 10, 65, 31, 45, 5]);
    showMTBarChart(["8/1", "8/2", "8/3", "8/4", "8/5"], [5, 20, 36, 10, 6], [8, 70, 16, 12, 9]);
    showGanttChart("StateGantt");
}

const getDailyData = mid => {
    console.log("mid=", mid);
    var category = [];
    var humiArr = [];
    var tempArr = [];
    // 生成今日0点时间
    const todayStart = new Date();
    todayStart.setHours(0, 0, 0, 0);

    // 生成明日0点时间
    const tomorrowStart = new Date(todayStart);
    tomorrowStart.setDate(todayStart.getDate() + 1);

    service.get("http://10.9.0.12:1880/GetTHShorthistoryForTH", {
        params: {
            Location: mid,
            TimeB: formatDateTime(todayStart),
            TimeE: formatDateTime(tomorrowStart)
        }
    }).then(res => {
        var hisTemp = [];

        if (res.Time && res.Time.length) {
            for (var i = 0; i < res.Time.length; i++) {
                hisTemp.push({
                    Time: res.Time[i],
                    Humi: res.Humi[i],
                    Temp: res.Temp[i]
                });
            }
            hisTemp = filterHourlyData(hisTemp);
            category = hisTemp.map(item => item.Hour);
            humiArr = hisTemp.map(item => item.Humi);
            tempArr = hisTemp.map(item => item.Temp);
            showChart(category, humiArr, tempArr);
        }
    }).catch(error => {
        console.error("数据获取失败:", error);
    });
};

//設備利用率折线图
const showEfficiencyLineChart = (category, humiArr, humiArr2) => {
    if (lineChartInstance1) {
        lineChartInstance1.dispose();
        lineChartInstance1 = null;
    }
    // 延迟确保DOM渲染完成
    setTimeout(() => {
        var obj = document.getElementById("efficiencyLine");
        lineChartInstance1 = echarts.init(obj, 'dark');

        const option = {
            // 标题组件，包含主标题和副标题
            title: {
                // text: '設備利用率',
                // left: '5%',
                // top: '5%',
                // //left: 'center', // 水平居中
                // textStyle: {
                //     fontSize: '1vw', // 将字体大小设置为14像素（你可以根据需要调整这个值）
                //     color: '#ffffff'
                // }
            },
            // 提示框组件
            tooltip: {
                trigger: 'axis' // 坐标轴触发
            },
            // 图例组件，展现不同系列的标记、颜色和名字
            legend: {
                //data: ['趋势数据']
            },
            // 直角坐标系内绘图网格
            grid: {
                left: '5%',
                right: '5%',
                bottom: '0%',
                top: '15%',
                containLabel: true
            },
            // 直角坐标系 grid 中的 x 轴
            xAxis: {
                type: 'category', // 类目轴，适用于离散的类目数据
                boundaryGap: false, // 坐标轴两边留白策略
                data: category, // 类目数据
                axisLabel: {
                    //color: 'dodgerblue' // 设置Y轴字体颜色为蓝色
                    color: 'white' // 设置Y轴字体颜色为蓝色
                },
                axisLine: {
                    lineStyle: {
                        color: 'white' // 设置Y轴线条颜色为白色
                    }
                }
            },
            // 直角坐标系 grid 中的 y 轴
            yAxis: {
                type: 'value', // 数值轴，适用于连续数据
                axisLabel: {
                    //color: 'dodgerblue' // 设置Y轴字体颜色为蓝色
                    color: 'white' // 设置Y轴字体颜色为蓝色
                },
                axisLine: {
                    lineStyle: {
                        color: 'white' // 设置Y轴线条颜色为白色
                    }
                }
            },
            backgroundColor: 'rgba(0, 128, 255, 0)',
            // 图表的核心部分，用于定义图表中显示的具体数据和样式
            series: [
                {
                    //name: '趋势数据',
                    type: 'line', // 线图
                    data: humiArr, // 系列中的数据值
                    // 折线图的样式
                    lineStyle: {
                        width: 2 // 折线宽度
                    },
                    // 折线拐点的样式
                    itemStyle: {
                        emphasis: {
                            borderWidth: 2,
                            shadowOffsetX: 0,
                            shadowOffsetY: 10,
                            shadowBlur: 15,
                            symbolSize: 30 // 拐点大小
                        }
                    },
                    smooth: true, // 折线是否平滑
                },
                {
                    //name: '趋势数据',
                    type: 'line', // 线图
                    data: humiArr2, // 系列中的数据值
                    // 折线图的样式
                    lineStyle: {
                        width: 2 // 折线宽度
                    },
                    // 折线拐点的样式
                    itemStyle: {
                        emphasis: {
                            borderWidth: 2,
                            shadowOffsetX: 0,
                            shadowOffsetY: 10,
                            shadowBlur: 15,
                            symbolSize: 30 // 拐点大小
                        }
                    },
                    smooth: true // 折线是否平滑
                }
            ]
        };

        lineChartInstance1.setOption(option);

        // 窗口变化自适应
        window.addEventListener("resize", handleResize);
    }, 1);

};

//热输入趋势折线图
const showHeatInputLineChart = (category, humiArr) => {
    if (lineChartInstance2) {
        lineChartInstance2.dispose();
        lineChartInstance2 = null;
    }
    // 延迟确保DOM渲染完成
    setTimeout(() => {
        var obj = document.getElementById("heatInputLine");
        lineChartInstance2 = echarts.init(obj, 'dark');
        const option = {
            // 标题组件，包含主标题和副标题
            title: {
                // show: '热输入趋势',
                // text: title,
                // left: '5%',
                // top: '5%',
                // //left: 'center', // 水平居中
                // textStyle: {
                //     fontSize: '1vw', // 将字体大小设置为14像素（你可以根据需要调整这个值）
                //     color: '#ffffff'
                // }
            },
            // 提示框组件
            tooltip: {
                trigger: 'axis' // 坐标轴触发
            },
            // 图例组件，展现不同系列的标记、颜色和名字
            legend: {
                //data: ['趋势数据']
            },
            // 直角坐标系内绘图网格
            grid: {
                left: '5%',
                right: '5%',
                bottom: '0%',
                top: '15%',
                containLabel: true
            },
            // 直角坐标系 grid 中的 x 轴
            xAxis: {
                type: 'category', // 类目轴，适用于离散的类目数据
                boundaryGap: false, // 坐标轴两边留白策略
                data: category, // 类目数据
                axisLabel: {
                    //color: 'dodgerblue' // 设置Y轴字体颜色为蓝色
                    color: 'white' // 设置Y轴字体颜色为蓝色
                },
                axisLine: {
                    lineStyle: {
                        color: 'white' // 设置Y轴线条颜色为白色
                    }
                }
            },
            // 直角坐标系 grid 中的 y 轴
            yAxis: {
                type: 'value', // 数值轴，适用于连续数据
                axisLabel: {
                    //color: 'dodgerblue' // 设置Y轴字体颜色为蓝色
                    color: 'white' // 设置Y轴字体颜色为蓝色
                },
                axisLine: {
                    lineStyle: {
                        color: 'white' // 设置Y轴线条颜色为白色
                    }
                }
            },
            backgroundColor: 'rgba(0, 128, 255, 0)',
            // 图表的核心部分，用于定义图表中显示的具体数据和样式
            series: [
                {
                    //name: '趋势数据',
                    type: 'line', // 线图
                    data: humiArr, // 系列中的数据值
                    // 折线图的样式
                    lineStyle: {
                        width: 2 // 折线宽度
                    },
                    // 折线拐点的样式
                    itemStyle: {
                        emphasis: {
                            borderWidth: 2,
                            shadowOffsetX: 0,
                            shadowOffsetY: 10,
                            shadowBlur: 15,
                            symbolSize: 30 // 拐点大小
                        }
                    },
                    smooth: true, // 折线是否平滑
                    areaStyle: {
                        opacity: 0.5
                    } // 开启面积填充
                }
            ]
        };
        lineChartInstance2.setOption(option);

        // 窗口变化自适应
        window.addEventListener("resize", handleResize);
    }, 1);

};

//當日設備生產工況趨勢折线图
const showEfficiencyLineChart2 = (category, humiArr, humiArr2) => {
    if (lineChartInstance3) {
        lineChartInstance3.dispose();
        lineChartInstance3 = null;
    }
    // 延迟确保DOM渲染完成
    setTimeout(() => {
        var obj = document.getElementById("efficiencyLine2");
        lineChartInstance3 = echarts.init(obj, 'dark');
        const option = {
            // 标题组件，包含主标题和副标题
            title: {
                text: "當日設備生產工況趨勢",
                left: '5%',
                top: '5%',
                //left: 'center', // 水平居中
                textStyle: {
                    fontSize: '1vw', // 将字体大小设置为14像素（你可以根据需要调整这个值）
                    color: '#ffffff'
                }
            },
            // 提示框组件
            tooltip: {
                trigger: 'axis' // 坐标轴触发
            },
            // 图例组件，展现不同系列的标记、颜色和名字
            legend: {
                //data: ['趋势数据']
            },
            // 直角坐标系内绘图网格
            grid: {
                left: '5%',
                right: '8%',
                bottom: '5%',
                top: '20%',
                containLabel: true
            },
            // 直角坐标系 grid 中的 x 轴
            xAxis: {
                type: 'category', // 类目轴，适用于离散的类目数据
                boundaryGap: false, // 坐标轴两边留白策略
                data: category, // 类目数据
                axisLabel: {
                    //color: 'dodgerblue' // 设置Y轴字体颜色为蓝色
                    color: 'white' // 设置Y轴字体颜色为蓝色
                },
                axisLine: {
                    lineStyle: {
                        color: 'white' // 设置Y轴线条颜色为白色
                    }
                }
            },
            // 直角坐标系 grid 中的 y 轴
            yAxis: {
                type: 'value', // 数值轴，适用于连续数据
                axisLabel: {
                    //color: 'dodgerblue' // 设置Y轴字体颜色为蓝色
                    color: 'white' // 设置Y轴字体颜色为蓝色
                },
                axisLine: {
                    lineStyle: {
                        color: 'white' // 设置Y轴线条颜色为白色
                    }
                }
            },
            backgroundColor: 'rgba(0, 128, 255, 0)',
            // 图表的核心部分，用于定义图表中显示的具体数据和样式
            series: [
                {
                    //name: '趋势数据',
                    type: 'line', // 线图
                    data: humiArr, // 系列中的数据值
                    // 折线图的样式
                    lineStyle: {
                        width: 2 // 折线宽度
                    },
                    // 折线拐点的样式
                    itemStyle: {
                        emphasis: {
                            borderWidth: 2,
                            shadowOffsetX: 0,
                            shadowOffsetY: 10,
                            shadowBlur: 15,
                            symbolSize: 30 // 拐点大小
                        }
                    },
                    smooth: true, // 折线是否平滑
                },
                {
                    //name: '趋势数据',
                    type: 'line', // 线图
                    data: humiArr2, // 系列中的数据值
                    // 折线图的样式
                    lineStyle: {
                        width: 2 // 折线宽度
                    },
                    // 折线拐点的样式
                    itemStyle: {
                        emphasis: {
                            borderWidth: 2,
                            shadowOffsetX: 0,
                            shadowOffsetY: 10,
                            shadowBlur: 15,
                            symbolSize: 30 // 拐点大小
                        }
                    },
                    smooth: true // 折线是否平滑
                }
            ]
        };
        lineChartInstance3.setOption(option);

        // 窗口变化自适应
        window.addEventListener("resize", handleResize);
    }, 1);

};

//UPH柱状图
const showUPHBarChart = (xData, humiArr) => {
    if (barChartInstance1) {
        barChartInstance1.dispose();
        barChartInstance1 = null;
    }
    // 延迟确保DOM渲染完成
    setTimeout(() => {
        var obj = document.getElementById("UPHBar");
        barChartInstance1 = echarts.init(obj);

        const option = {
            // 标题组件，包含主标题和副标题
            title: {
                // text: "UPH",
                // left: '5%',
                // top: '5%',
                // //left: 'center', // 水平居中
                // textStyle: {
                //     fontSize: '1vw', // 将字体大小设置为14像素（你可以根据需要调整这个值）
                //     color: '#ffffff'
                // }
            },
            grid: {
                left: '5%',
                right: '5%',
                bottom: '0%',
                top: '15%',
                containLabel: true
            },
            tooltip: {},
            legend: {
                show: false
                //data: ['销量']
            },
            xAxis: {
                data: xData,
                axisLabel: {
                    //color: 'dodgerblue' // 设置Y轴字体颜色为蓝色
                    color: 'white' // 设置Y轴线条颜色为白色
                },
                axisLine: {
                    lineStyle: {
                        color: 'white' // 设置Y轴线条颜色为白色
                    }
                }
            },
            yAxis: {
                axisLabel: {
                    //color: 'dodgerblue' // 设置Y轴字体颜色为蓝色
                    color: 'white' // 设置Y轴线条颜色为白色
                },
                axisLine: {
                    lineStyle: {
                        color: 'white' // 设置Y轴线条颜色为白色
                    }
                }
            },
            series: [{
                name: '销量',
                type: 'bar',
                data: humiArr
            }]
        };

        barChartInstance1.setOption(option);

        // 窗口变化自适应
        window.addEventListener("resize", handleResize);
    }, 1);
}

//當日設備UPH統計柱状图
const showUPHBarChart2 = (xData, humiArr) => {
    if (barChartInstance2) {
        barChartInstance2.dispose();
        barChartInstance2 = null;
    }
    // 延迟确保DOM渲染完成
    setTimeout(() => {
        var obj = document.getElementById("UPHBar2");
        barChartInstance2 = echarts.init(obj);
        const option = {
            // 标题组件，包含主标题和副标题
            title: {
                text: "當日設備UPH統計",
                left: '5%',
                top: '5%',
                //left: 'center', // 水平居中
                textStyle: {
                    fontSize: '1vw', // 将字体大小设置为14像素（你可以根据需要调整这个值）
                    color: '#ffffff'
                }
            },
            grid: {
                left: '5%',
                right: '8%',
                bottom: '5%',
                top: '20%',
                containLabel: true
            },
            tooltip: {},
            legend: {
                show: false
                //data: ['销量']
            },
            xAxis: {
                data: xData,
                axisLabel: {
                    //color: 'dodgerblue' // 设置Y轴字体颜色为蓝色
                    color: 'white' // 设置Y轴线条颜色为白色
                },
                axisLine: {
                    lineStyle: {
                        color: 'white' // 设置Y轴线条颜色为白色
                    }
                }
            },
            yAxis: {
                axisLabel: {
                    //color: 'dodgerblue' // 设置Y轴字体颜色为蓝色
                    color: 'white' // 设置Y轴线条颜色为白色
                },
                axisLine: {
                    lineStyle: {
                        color: 'white' // 设置Y轴线条颜色为白色
                    }
                }
            },
            series: [{
                name: '销量',
                type: 'bar',
                data: humiArr
            }]
        };
        barChartInstance2.setOption(option);

        // 窗口变化自适应
        window.addEventListener("resize", handleResize);
    }, 1);
}

//平均修复时间/平均故障間隔時間
const showMTBarChart = (xData, humiArr, humiArr2) => {
    if (barChartInstance3) {
        barChartInstance3.dispose();
        barChartInstance3 = null;
    }
    // 延迟确保DOM渲染完成
    setTimeout(() => {
        var obj = document.getElementById("MTBar");
        barChartInstance3 = echarts.init(obj);

        // 配置项
        var option = {
            // 标题组件，包含主标题和副标题
            title: {
                text: "平均修复时间/平均故障間隔時間",
                left: '5%',
                top: '5%',
                //left: 'center', // 水平居中
                textStyle: {
                    fontSize: '1vw', // 将字体大小设置为14像素（你可以根据需要调整这个值）
                    color: '#ffffff'
                }
            },
            grid: {
                left: '6%',
                right: '6%',
                bottom: '6%',
                top: '30%',
                containLabel: true
            },
            tooltip: {},
            legend: {
                data: ['MTTR', 'MTBF'],
                right: '5%',
                top: '15%',
                textStyle: {
                    fontSize: '1vw', // 将字体大小设置为14像素（你可以根据需要调整这个值）
                    color: '#ffffff'
                }
            },
            xAxis: {
                data: xData,
                axisLabel: {
                    color: 'dodgerblue' // 设置Y轴字体颜色为蓝色
                },
                axisLine: {
                    lineStyle: {
                        color: 'white' // 设置Y轴线条颜色为白色
                    }
                }
            },
            yAxis: {
                axisLabel: {
                    color: 'dodgerblue' // 设置Y轴字体颜色为蓝色
                },
                axisLine: {
                    lineStyle: {
                        color: 'white' // 设置Y轴线条颜色为白色
                    }
                }
            },
            series: [{
                name: 'MTTR',
                type: 'bar',
                data: humiArr
            },
            {
                name: 'MTBF',
                type: 'bar',
                data: humiArr
            }]
        };

        barChartInstance3.setOption(option);

        // 窗口变化自适应
        window.addEventListener("resize", handleResize);
    }, 1);
}

//用能趋势柱状图
const showEnergyBarChart = () => {
    if (barChartInstance4) {
        barChartInstance4.dispose();
        barChartInstance4 = null;
    }
    // 延迟确保DOM渲染完成
    setTimeout(() => {
        var obj = document.getElementById("energyBar");
        barChartInstance4 = echarts.init(obj);

        const option = {
            // 标题组件，包含主标题和副标题
            title: {
                // text: '用能趋势',
                // left: '5%',
                // top: '5%',
                // //left: 'center', // 水平居中
                // textStyle: {
                //     fontSize: '1vw', // 将字体大小设置为14像素（你可以根据需要调整这个值）
                //     color: '#ffffff'
                // }
            },
            grid: {
                left: '5%',
                right: '5%',
                bottom: '0%',
                top: '15%',
                containLabel: true
            },
            tooltip: {},
            legend: {
                show: false
                //data: ['销量']
            },
            calculable: true,
            xAxis: {
                data: category.value,
                axisLabel: {
                    //color: 'dodgerblue' // 设置Y轴字体颜色为蓝色
                    color: 'white' // 设置Y轴线条颜色为白色
                },
                axisLine: {
                    lineStyle: {
                        color: 'white' // 设置Y轴线条颜色为白色
                    }
                }
            },
            yAxis: {
                axisLabel: {
                    //color: 'dodgerblue' // 设置Y轴字体颜色为蓝色
                    color: 'white' // 设置Y轴线条颜色为白色
                },
                axisLine: {
                    lineStyle: {
                        color: 'white' // 设置Y轴线条颜色为白色
                    }
                }
            },
            series: [{
                name: '电',
                type: 'bar',
                data: humiArr.value,
                markPoint: {
                    data: [
                        { type: 'max', name: 'Max' },
                        { type: 'min', name: 'Min' }
                    ]
                },
                markLine: {
                    data: [{ type: 'average', name: 'Avg' }],
                    label: {
                        color: 'white'
                    }
                }
            }]
        };

        barChartInstance4.setOption(option);

        // 窗口变化自适应
        window.addEventListener("resize", handleResize);
    }, 1);
}

//甘特图
const showGanttChart = (id) => {
    if (ganttChartInstance) {
        ganttChartInstance.dispose();
        ganttChartInstance = null;
    }
    // 延迟确保DOM渲染完成
    setTimeout(() => {
        var obj = document.getElementById(id);
        ganttChartInstance = echarts.init(obj);

        // 模拟数据：设备状态及时间段
        var states = [
            { column: "正常", start: "00:00", end: "06:00" },
            { column: "停机", start: "06:00", end: "08:00" },
            { column: "待机", start: "08:00", end: "12:00" },
            { column: "故障", start: "12:00", end: "14:00" },
            { column: "正常", start: "14:00", end: "20:00" },
            { column: "停机", start: "20:00", end: "24:00" }
        ];

        // 将时间转换为分钟数，用于 X ��
        function timeToMinutes(time) {
            var parts = time.split(':');
            return parseInt(parts[0]) * 60 + parseInt(parts[1]);
        }

        // 提取所有图例数据
        var legendData = Array.from(new Set(states.map(state => state.column)));

        // 配置甘特图的系列数据
        var seriesData = states.map(function (state) {
            return {
                name: state.column,
                value: [
                    state.column, // 列名
                    timeToMinutes(state.start), // 开始时间（分钟）
                    timeToMinutes(state.end), // 结束时间（分钟）
                    state.column // 状态名称
                ]
            };
        });

        // 获取所有列的名称
        var columns = Array.from(new Set(states.map(state => state.column)));

        // 配置项
        var option = {
            title: {
                text: '当日设备状态甘特图',
                left: '5%',
                top: '5%',
                textStyle: {
                    fontSize: '1vw', // 将字体大小设置为14像素（你可以根据需要调整这个值）
                    color: '#ffffff'
                }
            },
            tooltip: {
                formatter: function (params) {
                    var start = Math.floor(params.value[1] / 60) + ':' + (params.value[1] % 60).toString().padStart(2, '0');
                    var end = Math.floor(params.value[2] / 60) + ':' + (params.value[2] % 60).toString().padStart(2, '0');
                    return `${params.value[3]}<br>时间段: ${start} - ${end}`;
                }
            },
            legend: {
                data: legendData,
                top: '10%',
                right: '10%',
                orient: 'vertical', // 图例垂直布局
                textStyle: {
                    fontSize: '1vw'
                },
                selected: legendData.reduce((obj, key) => {
                    obj[key] = true; // 默认启用所有图例
                    return obj;
                }, {})
            },
            xAxis: {
                type: 'value',
                min: 0,
                max: 1440, // 一天的总分钟数
                splitNumber: 0,
                axisLabel: {
                    formatter: function (value) {
                        var hours = Math.floor(value / 60);
                        var minutes = value % 60;
                        return hours + ':' + minutes.toString().padStart(2, '0');
                    }
                }
            },
            yAxis: {
                type: 'category',
                data: columns, // 列名作为 Y 轴
                inverse: true,
                axisLabel: {
                    show: false
                }
            },
            grid: {
                left: '10%',
                right: '10%',
                bottom: '6%',
                containLabel: true
            },
            series: [
                {
                    name: '设备状态',
                    type: 'custom',
                    renderItem: function (params, api) {
                        var categoryIndex = api.coord([0, api.value(0)])[1]; // 获取列对应的 Y 坐标
                        var start = api.coord([api.value(1), api.value(0)]);
                        var end = api.coord([api.value(2), api.value(0)]);
                        var height = api.size([0, 1])[1] * 0.6;

                        return {
                            type: 'rect',
                            shape: {
                                x: start[0],
                                y: categoryIndex - height / 2,
                                width: end[0] - start[0],
                                height: height
                            },
                            style: api.style()
                        };
                    },
                    data: seriesData,
                    encode: {
                        x: [1, 2],
                        y: 0
                    },
                    itemStyle: {
                        color: function (params) {
                            var colors = {
                                "正常": "#67c23a",
                                "停机": "#ffffff",
                                "待机": "#e6a23c",
                                "故障": "#f56c6c",
                            };
                            return colors[params.value[3]] || '#909399';
                        }
                    }
                }
            ]
        };

        ganttChartInstance.setOption(option);

        // 窗口变化自适应
        window.addEventListener("resize", handleResize);
    }, 1);
}

const getStatusText = (code) => {
    const statusMap = { 0: '正常', 1: '故障', 2: '待机' };
    return statusMap[code] || '未知';
}

const getCurrTime = () => {
    var i = new Date();
    var F = i.getFullYear();
    var C = i.getMonth() + 1;
    var E = i.getDate();
    var z = i.getDay();
    var A = i.getHours();
    var B = i.getMinutes();
    var D = i.getSeconds();
    var H = ['星期日', '星期一', '星期二', '星期三', '星期四', '星期五', '星期六'];
    var G;
    G = F + '-' + b(C) + '-' + b(E) + '  ' + H[z] + '  ' + b(A) + ':' + b(B) + ':' + b(D);
    currTime.value = G;
};

const b = (z) => {
    var i;
    if (z < 10) {
        i = '0' + z;
    } else {
        i = z;
    }
    return i;
};

// 日期格式化函数 ✅
const formatDateTime = date => {
    const pad = n => n.toString().padStart(2, "0"); // 补零工具函数

    return (
        `${date.getFullYear()}/${pad(date.getMonth() + 1)}/${pad(
            date.getDate()
        )} ` +
        `${pad(date.getHours())}:${pad(date.getMinutes())}:${pad(
            date.getSeconds()
        )}`
    );
};

// 根据窗口大小调整缩放比例
const updateScale = () => {
    const container = streetCanvas.value;
    if (container) {
        var canvasDiv = document.getElementById("canvasDiv");
        //canvasWidth.value = canvasDiv.offsetWidth;
        //canvasHeight.value = canvasDiv.offsetHeight;
        const containerWidth = canvasWidth.value; // 设计稿的宽度
        const containerHeight = canvasHeight.value; // 设计稿的高度

        const scaleX = (canvasDiv.offsetWidth) / containerWidth;
        const scaleY = (canvasDiv.offsetHeight) / containerHeight;

        // 取最小比例，保持等比缩放
        scale.value = Math.min(scaleX, scaleY);

        //container.style.transform = `scale(${scale.value})`;
        //container.style.transformOrigin = 'top left';
        // 设置 canvas 的实际分辨率
        //container.width = canvasDiv.offsetWidth - 20;
        //container.height = canvasDiv.offsetWidth - 10;

        // 居中对齐
        //container.style.left = `${(window.innerWidth - containerWidth * scale) / 2}px`;
        //container.style.top = `${(window.innerHeight - containerHeight * scale) / 2}px`;
        container.style.position = 'absolute';

        render();
    }
};

// 窗口调整监听
const handleResize = () => {
    updateScale();
    if (lineChartInstance1) lineChartInstance1.resize();
    if (lineChartInstance2) lineChartInstance2.resize();
    if (lineChartInstance3) lineChartInstance3.resize();
    if (barChartInstance1) barChartInstance1.resize();
    if (barChartInstance2) barChartInstance2.resize();
    if (barChartInstance3) barChartInstance3.resize();
    if (barChartInstance4) barChartInstance4.resize();
    if (ganttChartInstance) ganttChartInstance.resize();
};

// 组件销毁时清理资源
onBeforeUnmount(() => {
    window.removeEventListener("resize", handleResize);
    if (lineChartInstance1) lineChartInstance1.dispose();
    if (lineChartInstance2) lineChartInstance2.dispose();
    if (lineChartInstance3) lineChartInstance3.dispose();
    if (barChartInstance1) barChartInstance1.dispose();
    if (barChartInstance2) barChartInstance2.dispose();
    if (barChartInstance3) barChartInstance3.dispose();
    if (barChartInstance4) barChartInstance4.dispose();
    if (ganttChartInstance) ganttChartInstance.dispose();
});
</script>

<style scoped>
.el-main {
    padding: 0px;
}

.fold {
    position: absolute;
    top: 1.5%;
    right: 3.5vw;
    width: 1.5vw;
    height: 1.5vw;
    z-index: 10;
}

.fold .el-icon {
    height: 1.5vw;
    width: 1.5vw;
    color: aqua;
}

.fold .el-icon svg {
    height: 1.5vw;
    width: 1.5vw;
}

#canvasDiv {
    position: relative;
    overflow: hidden;
    /* border: 1px solid #87ceeb; */
    width: 100%;
    height: 100%;
}

canvas {
    display: block;
    margin: 0 auto;
    /* background-color: #061f3f; */
    /* background-color: #1e3f70; */
    /* 天空背景颜色 */
    cursor: grab;
    height: 100%;
    width: 100%;
}

canvas:active {
    cursor: grabbing;
}

.overlayDiv {
    position: absolute;
    width: 4vw;
    height: 4vw;
}

/* 粒子光效容器 */
.particles {
    position: absolute;
    bottom: 0;
    left: 0;
    pointer-events: none;
    overflow: hidden;
}

/* 粒子样式 */
.particles span {
    position: absolute;
    bottom: 0;
    display: block;
    width: 5px;
    height: 5px;
    background: rgba(255, 255, 255, 0.8);
    border-radius: 50%;
    animation: particle-move 3s linear infinite;
    opacity: 0;
    transform: rotate(45deg);
    transform-origin: top left;
    /* 以左上角为旋转中心 */
}

/* 随机粒子位置和动画延时 */
.particles span:nth-child(1) {
    left: 10%;
    animation-delay: 0s;
}

.particles span:nth-child(2) {
    left: 20%;
    animation-delay: 0.2s;
}

.particles span:nth-child(3) {
    left: 30%;
    animation-delay: 0.4s;
}

.particles span:nth-child(4) {
    left: 40%;
    animation-delay: 0.6s;
}

.particles span:nth-child(5) {
    left: 50%;
    animation-delay: 0.8s;
}

.particles span:nth-child(6) {
    left: 60%;
    animation-delay: 1s;
}

.particles span:nth-child(7) {
    left: 70%;
    animation-delay: 1.2s;
}

.particles span:nth-child(8) {
    left: 80%;
    animation-delay: 1.4s;
}

.particles span:nth-child(9) {
    left: 90%;
    animation-delay: 1.6s;
}

.particles span:nth-child(10) {
    left: 15%;
    animation-delay: 1.8s;
}

.particles span:nth-child(11) {
    left: 35%;
    animation-delay: 2s;
}

.particles span:nth-child(12) {
    left: 55%;
    animation-delay: 2.2s;
}

.particles span:nth-child(13) {
    left: 75%;
    animation-delay: 2.4s;
}

/* 粒子动画 */
@keyframes particle-move {
    0% {
        transform: translateY(0);
        opacity: 1;
    }

    50% {
        opacity: 1;
    }

    100% {
        transform: translateY(-300px);
        opacity: 0;
    }
}

:deep(.el-overlay) {
    position: static !important;
    /* 取消固定定位 */
}

:deep(.btt) {
    background-color: rgba(7, 32, 65, 0.8);
}

:deep(.ltr) {
    background-color: transparent;
}

:deep(.rtl) {
    background-color: transparent;
}

:deep(.btt .el-drawer__header) {
    margin-bottom: 0px;
    padding: 0px;
}

:deep(.ltr .el-drawer__header) {
    margin-bottom: 2vw;
    padding: 0px;
}

:deep(.rtl .el-drawer__header) {
    margin-bottom: 2vw;
    padding: 0px;
}

:deep(.btt .el-drawer__body) {
    padding: 10px 10px 0px 10px;
}

:deep(.ltr .el-drawer__body) {
    padding-left: 0px;
    padding-right: 0px;
    padding-bottom: 0px;
}

:deep(.rtl .el-drawer__body) {
    padding-left: 0px;
    padding-right: 0px;
    padding-bottom: 0px;
}

:deep(.el-drawer.btt) {
    /* left: 25%;
    right: 25%;
    width: 50%; */
    text-align: center;
}

:deep(.el-drawer) {
    box-shadow: none;
}


:deep(.border) {
    padding: 2px;
    width: 5vw;
    border: 1px dashed #87ceeb;
}

.info {
    position: absolute;
    font-size: 0.5vw;
    padding: 5px;
    text-align: center;
    line-height: 0.8vw;
    width: 6vw;
    color: #f1e7e7;
    pointer-events: none;
    /* 禁止鼠标事件 */
}

.colTitle {
    text-align: left;
    font-weight: bold;
    padding-left: 5px;
    font-size: 0.6vw;
    color: #ffffff;
}

.colCss {
    color: aquamarine;
    text-align: left;
}

:deep(.el-overlay-dialog) {
    text-align: center;
    backdrop-filter: blur(1px);
    /* 设置背景模糊程度 */
    background-color: rgba(90, 87, 87, 0.3);
    /* 设置遮罩层的半透明背景 */
}

:deep(.el-dialog) {
    background: url('@/assets/imgs/background2.png') no-repeat;
    background-size: cover;
    /* 或者使用 contain, cover, 100% 100% 等 */
    background-position: center;
    /* 背景图居中 */
    height: 99.5vh;
    /* 高度填充视口 */
    top: 0;
    /* 从顶部开始 */
    margin: 0;
    /* 移除默认的居中间距 */
    width: 70%;
    display: inline-block;
    text-align: left;
    color: #ffffff;

}

.f18 {
    font-size: 18px;
}

.f20 {
    font-size: 20px;
}

.fwb {
    font-weight: bold;
}

.h100 {
    height: 100%;
}

.h95 {
    height: 95%;
}

.h90 {
    height: 90%;
}

.h85 {
    height: 85%;
}

.h80 {
    height: 80%;
}

.w100 {
    width: 100%;
}

.tc {
    text-align: center;
}

.vam {
    vertical-align: middle;
}


::v-deep(.el-table th) {
    border: 1px solid skyblue !important;
    background-color: rgb(6, 33, 59, 0.3) !important;
}

::v-deep(.el-table td,
    .el-table th.is-leaf,
    .el-table--group,
    .el-table--border,
    .el-table--border td,
    .el-table--border th,
    .el-table__body-wrapper .el-table--border.is-scrolling-left~.el-table__fixed,
    .el-table--border th.el-table__cell) {
    border: 1px solid skyblue !important;
}


/* 设置el-table的背景色为透明 */
:deep(.el-table) {
    background-color: transparent !important;
    border: 1px solid skyblue !important;
}

/* 设置el-table-body的背景色为透明，以确保整个表格背景都是透明的 */
:deep(.el-table__body-wrapper .el-table__body) {
    background-color: transparent !important
}

/* 如果你使用了带边框的表格，可能还需要设置el-table--border的背景色为透明 */
:deep(.el-table--border::before,
    .el-table--border::after,
    .el-table__header-wrapper::before,
    .el-table__header-wrapper::after,
    .el-table__body-wrapper::before,
    .el-table__body-wrapper::after) {
    background-color: transparent !important;
}

/* 设置表头（thead）的背景色为透明，如果需要的话 */
:deep(.el-table__cell, .el-table__header-wrapper thead) {
    background-color: rgb(6, 33, 59, 0.3) !important;
    color: #fff;

}

/* 设置表格单元格（tr, td, th）的背景色为透明 */
:deep(.el-table tr, .el-table td, .el-table th) {
    background-color: transparent !important;
    color: #fff;
}

/* 自定义悬停行的背景颜色 */
:deep(.el-table__row:hover) {
    background-color: #f0f9eb86 !important;
    /* 自定义背景颜色 */
}
</style>
