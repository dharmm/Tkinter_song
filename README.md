import tkinter as tk
import random
import time
import threading


def display_lyrics():
    lyrics = [
        "Tu mera koi na hoke bhi kuch lage...",
        "Kyun bechaini dil mein yeh tu bhar de...",
        "Tu mera koi na hoke bhi kuch lage...",
        "Apna bana le pyaar apna bana le...",
        "Dil ke nagar mein khwaab tu saja le...",
        "Apna bana le pyaar apna bana le..."
    ]

    colors = ["#FF6F61", "#FFB74D", "#81C784", "#64B5F6", "#BA68C8", "#E57373"]

    for i, line in enumerate( lyrics ):
        for char in line:
            text_area.insert( tk.END, char )
            text_area.tag_add( f"color{i}", f"end-{len( char )}c", tk.END )
            text_area.tag_config( f"color{i}", foreground = colors[i % len( colors )] )
            text_area.update()
            time.sleep( 0.05 )
        text_area.insert( tk.END, "\n\n" )
        time.sleep( 0.5 )


def create_heart():
    x = random.randint( 50, 650 )
    y = 500
    size = random.randint( 30, 60 )  # Big heart size

    heart = canvas.create_text( x, y, text = "❤️", font = ("Arial", size), fill = "red" )

    def move_up():
        nonlocal y
        for step in range( 50 ):
            y -= 5
            canvas.move( heart, 0, -5 )
            canvas.itemconfig( heart, font = ("Arial", size + step // 5) )
            time.sleep( 0.05 )
        canvas.delete( heart )

    threading.Thread( target = move_up, daemon = True ).start()


def animate_hearts():
    while animation_running:
        create_heart()
        time.sleep( 0.2 ) 


def start_song():
    global animation_running
    animation_running = True
    threading.Thread( target = animate_hearts, daemon = True ).start()
    threading.Thread( target = display_lyrics, daemon = True ).start()


root = tk.Tk()
root.title( "🎶 Apna Bana Le ❤️ - Arijit Singh" )
root.geometry( "700x600" )
root.configure( bg = "#101820" )

canvas = tk.Canvas( root, bg = "#101820", highlightthickness = 0 )
canvas.pack( fill = tk.BOTH, expand = True )

text_area = tk.Text( canvas, wrap = "word", font = ("Consolas", 16), bg = "#101820", fg = "white", bd = 0 )
text_area.place( x = 50, y = 100, width = 600, height = 350 )

title_label = tk.Label( root, text = "🎵 Apna Bana Le - Arijit Singh 🎵",
                        font = ("Helvetica", 20, "bold"), fg = "#FFD700", bg = "#101820" )
title_label.place( x = 130, y = 20 )

start_button = tk.Button( root, text = "💖 Start Song 💖", command = start_song,
                          font = ("Arial", 16, "bold"), bg = "#FFD700", fg = "#101820", relief = "flat", padx = 15,
                          pady = 5 )
start_button.place( x = 250, y = 480 )

animation_running = False

root.mainloop()
