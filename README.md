# Face Recognition Project

This project demonstrates a simple face recognition system using Python and the `face_recognition` library. The system can load an image, encode the faces in the image, and compare them with known faces to identify individuals.

## Features

- Load and encode faces from an image
- Identify faces in an unknown image
- Display the image with identified faces and their names
- Save the output image with rectangles and names drawn

## Installation

To run this project, you need to have Python installed along with the following libraries:

- `face_recognition`
- `Pillow`
- `matplotlib`

You can install the required libraries using pip:
    

    pip install face_recognition Pillow matplotlib
   

## Usage

1. **Load the Image and Encode Faces**

    The script loads an image and encodes the faces in the image. It also extracts the name from the image file name.

    ```python
    import face_recognition
    import os
    from PIL import Image, ImageDraw, ImageFont
    import matplotlib.pyplot as plt

    def load_image(image_path):
        print("loading image..")
        image = face_recognition.load_image_file(image_path)
        print("encoding image..")
        encoding = face_recognition.face_encodings(image)[0]
        print("encoding done")
        return encoding

    def get_name_from_path(image_path):
        print("getting name from path..")
        return os.path.splitext(os.path.basename(image_path))[0]

    # Path to the image
    image_path = "imrankhan.jpg"
    # Load the image and get the encoding
    encoding = load_image(image_path)

    # List to store known face encodings and names
    known_face_encodings = []
    known_face_names = []

    # Append the encoding and the name to the lists
    known_face_encodings.append(encoding)
    known_face_names.append(get_name_from_path(image_path))

    pillow_image = Image.open(image_path)
    # Display the image using matplotlib
    plt.figure(figsize=(8, 6))  
    plt.imshow(pillow_image)
    plt.axis('off')
    plt.show()

    # Print to verify
    print("name ", known_face_names)
    ```
![uploading-file](https://github.com/user-attachments/assets/c17207d1-1793-45e7-9128-a9613126ca51)

2. **Identify Faces in an Unknown Image**

    The script loads an unknown image, finds faces in the image, and compares them with the known faces.

    ```python
    import face_recognition
    import os
    from PIL import Image, ImageDraw, ImageFont
    import matplotlib.pyplot as plt

    # Load the unknown image
    print("loading unknown image..")
    unknown_image_path = "hello.jpg"
    unknown_image = face_recognition.load_image_file(unknown_image_path)

    # Find faces in the unknown image
    print("finding faces in unknown image..")
    face_locations = face_recognition.face_locations(unknown_image)
    face_encodings = face_recognition.face_encodings(unknown_image, face_locations)

    # Number of faces detected
    num_faces = len(face_locations)
    print(f"Number of faces detected: {num_faces}")

    # Convert to PIL format for drawing
    pil_image = Image.fromarray(unknown_image)
    draw = ImageDraw.Draw(pil_image)

    # Load a font
    font_size = 20  # Change this value to set the font size
    font = ImageFont.truetype("Arial.ttf", font_size)

    # Compare faces with known faces
    for (top, right, bottom, left), face_encoding in zip(face_locations, face_encodings):
        matches = face_recognition.compare_faces(known_face_encodings, face_encoding)
        name = "Unknown"

        # If a match was found, use the name of the first match
        if True in matches:
            first_match_index = matches.index(True)
            name = known_face_names[first_match_index]

        # Draw a rectangle around the face
        draw.rectangle(((left, top), (right, bottom)), outline="red", width=3)

        # Calculate text size
        print("Calculate text size")
        text_bbox = draw.textbbox((left, bottom), name, font=font)
        text_width = text_bbox[2] - text_bbox[0]
        text_height = text_bbox[3] - text_bbox[1]

        # Draw a filled rectangle behind the text
        draw.rectangle(((left, bottom), (left + text_width, bottom + text_height + 10)), fill="red")

        # Draw the name below the face
        draw.text((left, bottom + 5), name, fill="white", font=font)

    # Display the image with names and rectangles
    # Display the image using matplotlib
    plt.imshow(pil_image)
    plt.axis('off')
    plt.show()

    # Save the image
    print("saving image..")
    pil_image.save("output_image.jpg")
    ```

## Output

The script displays the processed image with rectangles and names drawn around the faces and saves the image as `output_image.jpg`.

![saving](https://github.com/user-attachments/assets/176656fe-2371-469e-9347-dac843ebd892)
