# Facial Recognition Attendance System

This project demonstrates a high-accuracy facial recognition system built with Python. The project covers both the theory and implementation of facial recognition and then utilizes these techniques to build an attendance management system. This system uses a webcam to detect faces in real-time and logs attendance automatically in an Excel file.

## Features

- **Face Detection and Encoding**: Utilizes `face_recognition` to detect and encode faces for comparison.
- **Face Comparison**: Matches live-captured faces with pre-encoded images to verify identities.
- **Real-Time Attendance Logging**: Records the recognized individual's name and timestamp in an attendance log.
- **Automated Logging**: Attendance details are saved in a `.csv` file, making it easy to track who was present and at what time.

## Technologies Used

- **OpenCV**: For handling webcam video capture and displaying image data.
- **face_recognition**: A powerful library built on top of dlib to handle face recognition tasks.
- **NumPy**: Used for numerical operations on image data.
- **Python Standard Libraries**: Libraries such as `os` for file management and `datetime` for timestamping.

## Installation

To get started with this project, follow these steps:

1. **Clone this repository**:
    ```bash
    git clone https://github.com/yourusername/Facial-Recognition-Attendance-System.git
    cd Facial-Recognition-Attendance-System
    ```

2. **Install Required Libraries**:
    Make sure you have Python 3.x installed. Then install the necessary dependencies:
    ```bash
    pip install opencv-python numpy face_recognition
    ```

3. **Prepare Images**:
    - Place images of individuals (for whom attendance needs to be tracked) in a folder named `ImagesAttendance`.
    - Each image file should be named with the individual’s name (e.g., `John_Doe.jpg`).

## Usage

1. **Run the Project**:
    ```bash
    python main.py
    ```
2. **Face Recognition and Attendance Logging**:
    - The webcam will open, and the system will begin detecting faces.
    - Recognized faces will have a bounding box drawn around them with their name displayed.
    - Each recognized individual’s name and the current time will be saved to `Attendance.csv`.

## Code Overview

### Key Sections

- **Face Encoding**:
    ```python
    def findEncodings(images):
        encodeList = []
        for img in images:
            img = cv2.cvtColor(img, cv2.COLOR_BGR2RGB)
            encode = face_recognition.face_encodings(img)[0]
            encodeList.append(encode)
        return encodeList
    ```
    - Encodes each face image in `ImagesAttendance` and stores it for comparison.

- **Real-Time Attendance Logging**:
    ```python
    def markAttendance(name):
        with open('Attendance.csv', 'r+') as f:
            myDataList = f.readlines()
            nameList = [line.split(',')[0] for line in myDataList]
            if name not in nameList:
                now = datetime.now()
                dtString = now.strftime('%H:%M:%S')
                f.writelines(f'\n{name}, {dtString}')
    ```
    - Checks if a name has already been recorded for the current session; if not, logs it with a timestamp.

- **Webcam Face Detection**:
    ```python
    cap = cv2.VideoCapture(0)
    while True:
        success, img = cap.read()
        imgS = cv2.resize(img, (0, 0), None, 0.25, 0.25)
        imgS = cv2.cvtColor(imgS, cv2.COLOR_BGR2RGB)
        ...
    ```
    - Captures video from the webcam and processes each frame to detect and recognize faces in real-time.

## How it Works

1. **Face Detection and Encoding**:
   - When the program starts, it loads pre-stored images from `ImagesAttendance`, encodes each face, and stores these encodings.

2. **Real-Time Recognition**:
   - As the webcam captures live footage, each frame is processed to detect faces. Each detected face is encoded and compared with the pre-stored encodings to find matches.

3. **Attendance Marking**:
   - When a match is found, the person’s name is displayed on the screen and logged in `Attendance.csv` with the current timestamp.

## Example Usage

1. **Add images** of people in the `ImagesAttendance` folder.
2. **Run the script**. The system will recognize faces and record attendance automatically.

## Future Improvements

- Enhance accuracy in low-light or crowded environments.
- Add GUI to manage the attendance system.
- Explore additional storage options, like integrating with databases or cloud storage for attendance records.

## License

This project is open-source and available under the MIT License.
