## Create a React drag and drop file upload component from scratch 🥊

 ## Introduction
There a numerous libraries out there for you to implement this same functionality but if you want to know how it works and want to minimize the bloated components and dependencies then here is the way to do it.
In this article, we'll learn how to create our own drag-and-drop component in React, and we'll use the HTML5 native DnD API for this.

## Prerequisites - What You Need To Know

To follow along, you should have a basic understanding of react hooks and functional components.

- [React hooks overview - official documentation](https://reactjs.org/docs/hooks-overview.html)

## Overview of the app we’ll build
The final code for the app is [here](https://stackblitz.com/edit/react-me7tka?file=src%2FFileUploaderDND.js)
The preview of the app is [here](https://react-me7tka.stackblitz.io/). And here's how it looks:

![react-drag-and-drop-folder-working.gif](https://cdn.hashnode.com/res/hashnode/image/upload/v1621535872796/KS7dvI5AN.gif)


## The drag-and-drop HTML5 API

How it work is quite simple an element will take the **draggable** role and another element will be the drop target or **drop zone.**

for draggable element, the available events include: ****

- `ondragstart` - this event fires when you start dragging the element
- `ondragend` - fires when the drag action is complete

On the other hand, for the drop area, you can use the following events:

- `ondragenter` - this event fires when the draggable element is moved into a drop area
- `ondragover` - this event fires when you drag an element over a drop area
- `ondragleave` - this is the opposite of `ondragenter`, and fires when the draggable element is pulled out of the drop area
- `ondrop` - this event fires when you drop the element into the drop area

## And now The React way

Here I used useReducer hook. It  takes in a reducer function and an initial state as input, and returns the current state and a dispatch function as output. 

### File Structure


![react-drag-and-drop-folder-structure.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1621535964652/9yt1BWDNh.png)

`FileUploaderDND.js`
~~~jsx
import React, { useEffect } from 'react';

export default function FileUploaderDND(props) {
  const state = {
    inDropZone: false,
    fileList: []
  };

  const reducer = (state, action) => {
    switch (action.type) {
      case 'AddToDropZone':
        return { ...state, inDropZone: action.inDropZone };
      case 'AddToList':
        return { ...state, fileList: state.fileList.concat(action.files) };
      default:
        return state;
    }
  };

  const [data, dispatch] = React.useReducer(reducer, state);

  const handleDragEnter = event => {
    event.preventDefault();
    dispatch({ type: 'AddToDropZone', inDropZone: true });
  };

  const handleDragOver = event => {
    event.preventDefault();
    event.dataTransfer.dropEffect = 'move';
    dispatch({ type: 'AddToDropZone', inDropZone: true });
  };

  const handleDrop = event => {
    event.preventDefault();

    let files = [...event.dataTransfer.files];
    let files_with_preview = [];

    files.map((file, index) => {
      file[`image_${index}`] = URL.createObjectURL(file);
      files_with_preview.push(file);
    });

    if (files) {
      dispatch({ type: 'AddToList', files });
      dispatch({ type: 'AddToDropZone', inDropZone: false });
    }
  };

  useEffect(() => {
    if (data.fileList[0]) {
      const latestImage = data.fileList[data.fileList.length - 1];
      let blob = latestImage.preview;
      let name = latestImage.name;
      let img = new Image();
      img.src = blob;

      let reader = new FileReader();
      reader.readAsDataURL(latestImage);
      reader.onloadend = function() {
        let base64data = reader.result;
        props.changeInputFile({
          name: name,
          file: base64data,
          width: img.width,
          height: img.height
        });
      };
    }
  }, [data]);

  return (
    <div
      id="fileuploaderdnd-container"
      className="fileuploaderdnd-container"
      onDrop={event => handleDrop(event)}
      onDragOver={event => handleDragOver(event)}
      onDragEnter={event => handleDragEnter(event)}
    >
      <div className="fileuploaderdnd-container-button">
        <div className="fileuploaderdnd-container-text">
          drag and drop an image here to see output 👉🏼
        </div>
      </div>
    </div>
  );
}
~~~

`App.js`

~~~jsx
import React, { useState } from 'react';
import FileUploaderDND from './FileUploaderDND';
import './style.css';

export default function App() {
  const [image, setImage] = useState('');

  const setImageAction = img => {
    console.log(img);
    setImage(img);
  };

  return (
    <>
      <h1>File Uploader Drag and Drop</h1>
      <div className="container">
        <FileUploaderDND changeInputFile={setImageAction} />
        {image ? (
          <img
            className="placeholder"
            src={image.file}
            width={250}
            height={250}
          />
        ) : (
          <div className="placeholder" />
        )}
      </div>
      <div className="footer">
        <a href="https://www.milindsoorya.site">milindsoorya.site</a>
      </div>
    </>
  );
}
~~~

`style.scss`

~~~jsx
h1,
p {
  font-family: Lato;
  text-align: center;
}

.container {
  text-align: center;
  display: flex;
  width: 100%;
  justify-content: space-evenly;
}

.placeholder {
  height: 250px;
  width: 250px;
  background-color: pink;
  padding: 20px;
}

.fileuploaderdnd-container {
  height: 250px;
  width: 250px;

  background-color: #87879231;
  padding: 20px;
}

.input-img-file-file {
  display: none;
}

.fileuploaderdnd-container-button {
  position: relative;
  top: 180px;
  display: grid;
  place-items: center;
}

.fileuploaderdnd-container-text {
  font-size: 25px;
  color: black;
  opacity: 75%;
  margin-top: 12px;
}

.button {
  background-color: #4caf50;
  border: none;
  color: white;
  padding: 15px 32px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
  margin: 4px 2px;
  cursor: pointer;
}

.footer {
  width: 100%;
  text-align: center;
  margin-top: 50px;
}

@media (max-width: 600px) {
  .container {
    flex-direction: column;
    align-items: center;
  }
}
~~~

Thanks for reading have a great day. 🎊
