# Instagram Downloader

This project is designed to download Instagram highlights using the Instaloader library.

## Setup

1. Clone the repository:

   ```bash
   git clone https://github.com/Leila2024/Instagram_Downloader.git
   ```

2. Navigate to the project directory:

   ```bash
   cd Instagram_Downloader
   ```

3. Install the required Python packages:

   ```bash
   pip install -r requirements.txt
   ```

4. Create a `.env` file in the root of the project and add your Instagram credentials:

   ```plaintext
   INSTA_USERNAME=your_instagram_username
   INSTA_PASSWORD=your_instagram_password
   ```

5. Run the script:

   ```bash
   python src/main.py
   ```

## Features

- Logs in to Instagram using your credentials.
- Downloads all highlights from a specified Instagram profile.

## Requirements

- Python 3.x
- `instaloader`
- `python-dotenv`
```

### 7. **Create the `src/main.py` Script**

Inside the `src` directory, create a `main.py` file with the provided script:

```python
import instaloader
import os
import logging
from dotenv import load_dotenv

# Load environment variables from a .env file
load_dotenv()

# Configure logging
logging.basicConfig(level=logging.INFO, format='%(asctime)s - %(levelname)s - %(message)s')

# Create an instance of Instaloader
instance = instaloader.Instaloader()

# Login credentials
username = os.getenv('INSTA_USERNAME')
password = os.getenv('INSTA_PASSWORD')

try:
    instance.login(user=username, passwd=password)
    logging.info("Logged in successfully")
except Exception as e:
    logging.error(f"Failed to login: {e}")
    exit(1)

# Target profile
profile_name = 'gwenstefani'
try:
    profile = instaloader.Profile.from_username(instance.context, username=profile_name)
    logging.info(f"Profile '{profile_name}' loaded successfully")
except Exception as e:
    logging.error(f"Failed to load profile '{profile_name}': {e}")
    exit(1)

# Create output folder
output_folder = os.path.join(os.getcwd(), 'downloaded_highlights')
if not os.path.exists(output_folder):
    os.makedirs(output_folder)
    logging.info(f"Created output folder: {output_folder}")

# Download highlights
try:
    for highlight in instance.get_highlights(user=profile):
        for item in highlight.get_items():
            instance.download_storyitem(item, '{}/{}'.format(highlight.owner_username, highlight.title))
            logging.info(f"Downloaded highlight item: {item.mediaid}")
except Exception as e:
    logging.error(f"Failed to download highlights: {e}")
```

### 8. **Push Changes to GitHub**

After setting up everything locally, you can push your changes to GitHub:

```bash
git add .
git commit -m "Initial setup for Instagram Downloader project"
git push origin main
```

Now your repository is ready and set up on GitHub. You or anyone else can clone it, install dependencies, and run the script as described in the README.
