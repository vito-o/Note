## css动画

- transition: 过度动画
 - transition-property:属性
 - transition-duration:间隔
 - transition-timing-function:曲线
 - transition-delay:延迟
 - 常用钩子: transitionend


- animation / keyframes
 - animation-name:动画名称，对于@keyframes
 - animation-duation:间隔
 - animation-timing-function:曲线
 - animation-delay:延迟
 - animation-iteration-count:次数
   - infinite:循环动画
 - animation-direction:方向
   - alternate:反向播放
 -animation-fill-mode:静止模式
   - forwards:停止时，保留最后一帧
   - backwards:停止时，回到第一帧
   - both：同时运用forwards / backwards
 - 常用钩子：animationend
- 动画属性：尽量使用动画属性进行动画，能拥有较好的性能表现
 - translate:移动
 - scale：放大缩小
 - rotate 旋转
 - skew 扭曲
 - opacity 
 - color

