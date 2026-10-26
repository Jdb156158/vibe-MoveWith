# 制作步骤与复现


1. 核对源站目录、组件 HTML/JS 与演示画面，整理来源和分镜，见 [treatment.md](../film/treatment.md)。
2. 安装 Node.js、Python 3 / numpy、Chrome、FFmpeg；先进入 `film/` 目录，再运行 `npm install`。
3. `index.html` 包含五种控件的确定性模拟、Bento 布局和字幕；每个场景由绝对时间计算状态。预览页是影片时间轴，不是接受真实拖动的生产组件页面。
4. `python3 make_audio.py` 合成 20 秒、120 BPM、2/4 拍音轨。18 秒终止和弦，末尾 0.8 秒淡出。
5. `npm run stills` 导出九个 JPG。检查文字裁切、重排遮挡、指针位置、动作与结果，再修订。
6. `npm run render` 调用 `draw(i/60)` 导出 1,200 帧。默认使用 macOS Chrome 路径，其他平台通过 `CHROME_PATH` 指定。
7. `sh encode.sh` 生成 `drag-microinteractions-20s.mp4`：H.264 Main / yuv420p / BT.709、AAC 48kHz、每秒关键帧、faststart。
8. 检查实际 MP4 抽帧、完整播放、跳转与音频峰值。记录见 [影片 QA.md](../film/QA.md)。
9. 使用 huashu-motion 时，可运行 `om studio . --no-open` 预览时间轴，`om check .` 检查确定性，`om qa .` 检查突帧与节奏。


## 可操作网页

根目录 index.html 和 site/ 是响应真实输入的组件网页；film/ 保留确定性的影片源码。

- 看板和回收站使用 Pointer Events 与真实目标矩形命中，键盘提供等效操作。
- 预览内容按照逻辑视口宽度布局，并等比例缩放适配展示区。900 / 600px 为换列断点。
- SVG 关系网通过屏幕矩阵逆变换换算鼠标和触屏坐标，节点与连线同步，松手阻尼回弹。
- 浮窗使用同款程序绘制的循环山景视频，移动同一个 video 元素，保留播放进度；按释放位置吸附最近角落。
- 手机支持触屏拖动，控件以外仍可正常滚动页面；支持减少动态偏好。

## 发布

GitHub Pages 从 main 根目录发布，.nojekyll 保留静态资源，不需要 npm 构建。
本地运行：`python3 -m http.server 4858`，打开 `http://localhost:4858/`。
