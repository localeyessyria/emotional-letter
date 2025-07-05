# emotional-letter
import zipfile

import os



# Define the project structure

project_name = "emotional-letter-app"

base_path = f"/mnt/data/{project_name}"

os.makedirs(base_path, exist_ok=True)



# Pages (React-style routing assumed via React Router or similar)

pages = {

    "index.jsx": """\

import ArabicLetterPage from './components/ArabicLetterPage';

export default function App() {

  return <ArabicLetterPage />;

}

""",

    "view.jsx": """\

import ViewLetterPage from './components/ViewLetterPage';

export default function View() {

  return <ViewLetterPage />;

}

"""

}



# Components (already provided)

components = {

    "ArabicLetterPage.jsx": "PLACEHOLDER_FOR_MAIN_PAGE",

    "ViewLetterPage.jsx": "PLACEHOLDER_FOR_VIEW_PAGE"

}



# Package.json for React app

package_json = """\

{

  "name": "emotional-letter-app",

  "version": "1.0.0",

  "private": true,

  "scripts": {

    "dev": "vite",

    "build": "vite build",

    "preview": "vite preview"

  },

  "dependencies": {

    "react": "^18.2.0",

    "react-dom": "^18.2.0"

  },

  "devDependencies": {

    "vite": "^4.0.0",

    "@vitejs/plugin-react": "^4.0.0"

  }

}

"""



# Vite config

vite_config = """\

import { defineConfig } from 'vite'

import react from '@vitejs/plugin-react'



export default defineConfig({

  plugins: [react()],

  build: {

    outDir: 'dist'

  }

})

"""



# Create necessary files

with open(f"{base_path}/package.json", "w") as f:

    f.write(package_json)



with open(f"{base_path}/vite.config.js", "w") as f:

    f.write(vite_config)



# Create src folder and add pages

src_path = f"{base_path}/src"

components_path = os.path.join(src_path, "components")

os.makedirs(components_path, exist_ok=True)



for filename, content in pages.items():

    with open(os.path.join(src_path, filename), "w") as f:

        f.write(content)



# These will be replaced with full component code below

for filename in components:

    with open(os.path.join(components_path, filename), "w") as f:

        f.write(components[filename])



# Zip the whole project

zip_path = f"/mnt/data/{project_name}.zip"

with zipfile.ZipFile(zip_path, 'w') as zipf:

    for foldername, subfolders, filenames in os.walk(base_path):

        for filename in filenames:

            file_path = os.path.join(foldername, filename)

            zipf.write(file_path, os.path.relpath(file_path, base_path))



zip_path  # Return path to the zip file
