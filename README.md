# 1import cv2

def add_subtitle(video_path, subtitle_text, output_path):
    # 打开视频文件
    video = cv2.VideoCapture(video_path)

    # 检查视频文件是否打开成功
    if not video.isOpened():
        print("无法打开视频文件。")
        return

    # 获取视频的宽度和高度
    width = int(video.get(cv2.CAP_PROP_FRAME_WIDTH))
    height = int(video.get(cv2.CAP_PROP_FRAME_HEIGHT))

    # 获取视频的帧率
    fps = video.get(cv2.CAP_PROP_FPS)

    # 定义字幕的颜色、字体、字号、位置等参数
    subtitle_color = (255, 255, 255)  # 白色
    font = cv2.FONT_HERSHEY_SIMPLEX
    font_scale = 1.2
    subtitle_position = (30, height - 50)

    # 创建输出视频的写入器
    fourcc = cv2.VideoWriter_fourcc(*'mp4v')
    output_video = cv2.VideoWriter(output_path, fourcc, fps, (width, height))

    # 遍历视频的每一帧
    while video.isOpened():
        ret, frame = video.read()

        if ret:
            # 添加字幕到帧上
            cv2.putText(frame, subtitle_text, subtitle_position, font, font_scale, subtitle_color, 2)

            # 将帧写入输出视频
            output_video.write(frame)

            # 显示帧
            cv2.imshow('Video Subtitles', frame)

            # 按下 'q' 键退出
            if cv2.waitKey(1) & 0xFF == ord('q'):
                break
        else:
            break

    # 释放视频相关的资源
    video.release()
    output_video.release()
    cv2.destroyAllWindows()

# 调用函数添加字幕到视频
video_path = 'input_video.mp4'  # 输入视频路径
subtitle_text = '这是一个示例字幕'  # 字幕文本
output_path = 'output_video.mp4'  # 输出视频路径

add_subtitle(video_path, subtitle_text, output_path)
