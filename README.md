import cv2
import os

# Video settings
video_path = "your_video.mp4"
output_folder = "flipbook_frames"
output_pdf = "flipbook.pdf"
frame_interval = 5  # Capture every nth frame

# Create output folder
os.makedirs(output_folder, exist_ok=True)

# Load video
video = cv2.VideoCapture(video_path)
success, image = video.read()
count = 0

while success:
    if count % frame_interval == 0:
        # Save frame as image
        frame_file = os.path.join(output_folder, f"frame_{count}.jpg")
        cv2.imwrite(frame_file, image)
    success, image = video.read()
    count += 1

video.release()

# Convert frames to a PDF (requires PIL/Pillow)
from PIL import Image
images = []
for file_name in sorted(os.listdir(output_folder)):
    if file_name.endswith(".jpg"):
        img_path = os.path.join(output_folder, file_name)
        images.append(Image.open(img_path))

if images:
    images[0].save(output_pdf, save_all=True, append_images=images[1:], quality=100)

print(f"Flipbook PDF created as '{output_pdf}'")
