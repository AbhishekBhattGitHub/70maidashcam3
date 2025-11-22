import cv2
import os
import subprocess

def connect_known_wifi(ssid) -> bool:    
    try:
        print(f"Connecting to Wifi '{ssid}'...")
        subprocess.run(
            ["netsh", "wlan", "connect", f"name={ssid}"],
            check=True
        )
        return True
    except subprocess.CalledProcessError:
        print(f"Failed to connect to Wifi '{ssid}'. Make sure it is a known network.")
        return False
    
# RTSP URL for 70mai dash cam
rtsp_url = "rtsp://192.72.1.1:554/liveRTSP/av4" # RTSP/1.0"

def ping() -> bool:
    ip = "192.72.1.1"
    print("Pinging...")
    try:
        subprocess.check_output(
            ["ping", "-n", str(4), ip],
            stderr=subprocess.STDOUT,
            universal_newlines=True,
            creationflags=subprocess.CREATE_NO_WINDOW
        )
        return True
    except subprocess.CalledProcessError:
        return False


def main():
    while True:
        if connect_known_wifi("70mai_M200_d449"):
            while True:
                if ping():
                    while True:
                        print("Opening RTSP stream:", rtsp_url)
                        cap = cv2.VideoCapture(rtsp_url)
                        if cap.isOpened():
                            print("Opened the stream.")
                            cap.set(cv2.CAP_PROP_FRAME_WIDTH, 640)
                            cap.set(cv2.CAP_PROP_FRAME_HEIGHT, 360)
                            while True:
                                ret, frame = cap.read()
                                if ret:
                                    cv2.imshow("70mai Live Stream", frame)
                                    if cv2.waitKey(10) & 0xFF == ord('q'):
                                        return
                                else:
                                    print("Failed to get frame …")
                                    cap.release()
                                    cv2.destroyAllWindows()
                                    break
                        else:
                            print("Error: Cannot open stream.")
                            cap.release()
                            cv2.destroyAllWindows()
                            break

    cap.release()
    cv2.destroyAllWindows()

if __name__ == "__main__":
    main()
