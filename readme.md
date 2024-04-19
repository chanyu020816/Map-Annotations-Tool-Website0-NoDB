# Setup

```sh
docker build -t map_ano
docker run -p 8000:8000 --rm --mount src="$(pwd)",target=/website,type=bind map_ano
```


@app.route('/download_image', methods=['POST'])
def download_image():
    data = request.json
    image_data = data['image_data']
    image_name = data["image_name"]
    image_name = re.sub(r'\s+', '_', image_name)
    
    image_folder_path = "./Annotations/image/"
    output_path = os.path.join(image_folder_path, f'{image_name}.jpg')
    
    image_data = image_data.split(",")[1]  # Remove the prefix of Base64 encoding if present
    image_bytes = base64.b64decode(image_data)
    
    image = pilImage.open(io.BytesIO(image_bytes))
    image = image.convert('RGB')
    image.save(output_path)
    
    return jsonify({'message': 'Image Saved Successfully.'})