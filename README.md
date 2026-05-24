# 5a_Create_Socket_for_HTTP_for_webpage_upload_and_download
## AIM :
To write a PYTHON program for socket for HTTP for web page upload and download
## Algorithm

1.Start the program.
<BR>
2.Get the frame size from the user
<BR>
3.To create the frame based on the user request.
<BR>
4.To send frames to server from the client side.
<BR>
5.If your frames reach the server it will send ACK signal to client otherwise it will send NACK signal to client.
<BR>
6.Stop the program
<BR>
## Program 
```
import socket

def send_request(host, port, request):
    with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as s:
        s.connect((host, port))
        s.sendall(request)

        response = b""
        while True:
            data = s.recv(4096)
            if not data:
                break
            response += data

    return response.decode(errors='ignore')


def upload_file(host, port, filename):
    with open(filename, 'rb') as file:
        file_data = file.read()
        content_length = len(file_data)

        request = (
            f"POST /upload HTTP/1.1\r\n"
            f"Host: {host}\r\n"
            f"Content-Length: {content_length}\r\n"
            f"Content-Type: text/plain\r\n"
            f"\r\n"
        ).encode() + file_data

        response = send_request(host, port, request)

    return response


def download_file(host, port, filename):
    request = (
        f"GET /{filename} HTTP/1.1\r\n"
        f"Host: {host}\r\n"
        f"\r\n"
    ).encode()

    response = send_request(host, port, request)

    if '\r\n\r\n' in response:
        file_content = response.split('\r\n\r\n', 1)[1]

        with open("downloaded_" + filename, 'w') as file:
            file.write(file_content)

        print("File downloaded successfully.")
    else:
        print("Invalid response received.")


if __name__ == "__main__":

    # Local server
    host = 'localhost'
    port = 8080

    # Upload file
    upload_response = upload_file(host, port, 'example.txt')
    print("Upload response:\n", upload_response)

    # Download file
    download_file(host, port, 'example.txt')
```

## OUTPUT
<img width="1620" height="475" alt="Screenshot 2026-05-24 140123" src="https://github.com/user-attachments/assets/60484cf4-3fa6-46ee-bc92-2a23246ded00" />

## Result
Thus the socket for HTTP for web page upload and download created and Executed
