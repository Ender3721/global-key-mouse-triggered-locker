import tkinter as tk
from pynput import keyboard, mouse
import threading
import time
import os

# 全局变量
lock_triggered = False
keyboard_listener = None
mouse_listener = None

# 锁屏函数
def lock_screen():
    global lock_triggered
    if not lock_triggered:
        os.system('rundll32.exe user32.dll,LockWorkStation')
        lock_triggered = True
        # 停止监听器
        if keyboard_listener is not None:
            keyboard_listener.stop()
        if mouse_listener is not None:
            mouse_listener.stop()

# 监听键盘事件
def on_press(key):
    lock_screen()

# 监听鼠标事件
def on_move(x, y):
    lock_screen()

# 后台运行的主函数
def main(duration):
    global keyboard_listener, mouse_listener
    # 创建并启动键盘和鼠标监听线程
    keyboard_listener = keyboard.Listener(on_press=on_press)
    mouse_listener = mouse.Listener(on_move=on_move)

    keyboard_listener.start()
    mouse_listener.start()

    # 启动倒计时
    timer = threading.Timer(duration, lock_screen)
    timer.start()

    # 等待直到锁屏被触发
    timer.join()

# GUI 输入界面
def start_gui():
    def on_submit():
        duration = int(entry.get()) * 60
        root.destroy()
        main(duration)

    root = tk.Tk()
    root.title("锁屏计时器")

    tk.Label(root, text="请输入锁屏时间（分钟）:").pack()
    entry = tk.Entry(root)
    entry.pack()
    tk.Button(root, text="确定", command=on_submit).pack()

    root.mainloop()

if __name__ == "__main__":
    start_gui()
