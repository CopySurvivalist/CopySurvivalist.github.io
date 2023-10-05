<!DOCTYPE html>
<html>
<head>
    <title>Login Or Create Character</title>
</head>
<body>
    <h2>Create</h2>
    <form method="post" action="create.php">
        <label for="username">Username:</label>
        <input type="text" id="username" name="username" required>
        <br>
        <input type="submit" value="Create">
    </form>
    <h2>Character Count Form</h2>
    <form>
        <label for="inputField">Input Field:</label>
        <input type="text" id="inputField" name="inputField" onkeyup="showMatchingImage()">
    </form>
    <div id="imageContainer" style="display: none;">
        <a id="imageLink" href="#" target="_blank">
            <img id="displayedImage" alt="Image" width="650" height="600">
        </a>
    </div>
    <div id="feedbackMessage"></div>
    <script>
        // Fetch image data from the server when the page loads
        fetch('list_images.php')
            .then(response => response.json())
            .then(data => {
                var imageNames = data.images;
                var inputField = document.getElementById('inputField');
                var imageContainer = document.getElementById('imageContainer');
                var displayedImage = document.getElementById('displayedImage');
                var imageLink = document.getElementById('imageLink');
                var feedbackMessage = document.getElementById('feedbackMessage');
                var threshold = 2; // Change this to the desired character count threshold
                // Function to update the displayed image based on the input text
                function updateImage() {
                    var inputText = inputField.value.toLowerCase();
                    feedbackMessage.textContent = ''; // Clear any previous feedback message
                    if (inputText.length >= threshold) {
                        // Find the first matching image filename in the data
                        var matchingImage = imageNames.find(function (imageName) {
                            return imageName.includes(inputText);
                        });
                        if (matchingImage) {
                            // Extract the text after "$" symbol in the image name
                            var imageNameParts = matchingImage.split('$')[1];
                            var imageNameTextAfterDollar = imageNameParts.split('.')[0];
                            // Set the source of the image to the matching image
                            displayedImage.src = "web_images/" + matchingImage;
                            feedbackMessage.textContent = imageNameTextAfterDollar;
                            imageLink.href = "user_pages/" +imageNameTextAfterDollar + ".html"; // Navigate to the related HTML page
                            imageContainer.style.display = 'block';
                        } else {
                            imageContainer.style.display = 'none';
                            feedbackMessage.textContent = 'No matching image found.';
                        }
                    } else {
                        imageContainer.style.display = 'none';
                    }
                }
                // Call the updateImage function when the page loads
                updateImage();
                // Attach the updateImage function to the input field's keyup event
                inputField.addEventListener('keyup', updateImage);
            })
            .catch(error => {
                console.error('Error fetching image data:', error);
                feedbackMessage.textContent = 'Error fetching image data.';
            });
    </script>
</body>
</html>
