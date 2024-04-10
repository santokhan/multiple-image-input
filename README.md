# Multiple Image Input
How to handle multiple image import in react application?

```jsx
import React, { useEffect, useRef, useState } from 'react';
import { twMerge } from 'tailwind-merge';
import Button from '../../Button';
import ImagePreview from '../ImagePreview';
import MediaInputIcon from '../../icons/MediaInput';

/**
 * A component for selecting and displaying multiple images.
 * @param {object} props - Component props.
 * @param {Function} props.setValue - Function to update parent form state with selected files.
 * @param {string} [props.className=''] - Additional CSS classes to apply.
 * @returns {JSX.Element} MediaInput component.
 */
const MediaInput = ({ setValue, className = '' }) => {
  const [selectedFiles, setSelectedFiles] = useState([]); // State to store selected files
  const fileInputRef = useRef(null); // Ref for file input element
  const inputName = 'images'; // Name for the input field

  /**
   * Handles file selection from the input element.
   * @param {Event} e - The file input change event.
   */
  const handleFileSelect = (e) => {
    const inputFiles = e.target.files;
    setSelectedFiles((prevFiles) => [...prevFiles, ...inputFiles]);
  };

  /**
   * Removes a file from the selected files list.
   * @param {number} index - Index of the file to remove.
   */
  const handleRemoveImage = (index) => {
    setSelectedFiles((prevFiles) => {
      const updatedFiles = [...prevFiles];
      updatedFiles.splice(index, 1);
      return updatedFiles;
    });
  };

  // Update parent form state whenever selectedFiles change
  useEffect(() => {
    setValue(inputName, selectedFiles);
  }, [selectedFiles, setValue]);

  return (
    <div className={twMerge('w-full space-y-6', className)}>
      {/* File input drop zone */}
      <label
        className={twMerge(
          'border-2 border-dashed rounded-lg border-gray-300 bg-gray-50 p-6 py-8 lg:py-16 text-center w-full',
          'flex flex-col items-center relative'
        )}
      >
        <div className="mb-6">
          <MediaInputIcon />
        </div>
        <div className="flex flex-col items-center">
          <div className="text-lg font-semibold mb-1">
            Drag and drop images here
          </div>
          <div className="text-sm mb-6">
            Photos must be JPEG or PNG format and at least 2048x768
          </div>
          {/* Button to trigger file selection */}
          <Button variant="outline" withIcon={true} onClick={() => fileInputRef.current.click()}>
            Browse Files
          </Button>
        </div>
        {/* Hidden file input element */}
        <input
          name="media"
          type="file"
          accept="image/*"
          multiple
          ref={fileInputRef}
          className="absolute top-0 left-0 w-full h-full opacity-0 z-[1] bg-black"
          onChange={handleFileSelect}
          required
        />
      </label>

      {/* Image previews */}
      <div className="flex gap-4 flex-wrap">
        {selectedFiles.map((file, index) => (
          <ImagePreview
            key={index}
            src={URL.createObjectURL(file)}
            onRemove={() => handleRemoveImage(index)}
          />
        ))}
      </div>
    </div>
  );
};

export default MediaInput;
```

**Documentation Breakdown:**

- **Component Description**:
  - `MediaInput` is a React component that enables users to select and display multiple images. It consists of a drop zone for dragging and dropping images or clicking to select images.

- **Props**:
  - `setValue` (`Function`): A function passed from the parent component to update the form state with selected files.
  - `className` (`string`, optional): Additional CSS classes to customize the component's appearance.

- **State**:
  - `selectedFiles` (`Array`): State variable to hold an array of selected `File` objects.

- **Hooks & Ref**:
  - `useState`: Used to manage component state (e.g., `selectedFiles`).
  - `useRef`: Used to create a reference to the file input element (`fileInputRef`).

- **Functions**:
  - `handleFileSelect`: Event handler triggered when files are selected. It updates `selectedFiles` state with newly selected files.
  - `handleRemoveImage`: Function to remove a specific image from `selectedFiles` based on its index.

- **Lifecycle & Side Effects**:
  - `useEffect`: This hook is used to run side effects after rendering. It updates the parent form state (`setValue`) whenever `selectedFiles` changes.

- **Rendering**:
  - The component renders a drop zone (`label`) where users can click to select files or drag and drop them. It includes an invisible file input (`input`) that is triggered by the "Browse Files" button.
  - Selected images are displayed as previews using the `ImagePreview` component. Each preview has a remove button (`onRemove` function) to delete the associated image from `selectedFiles`.

This simplified `MediaInput` component is designed to be reusable and flexible, allowing easy integration into forms or other components that require multi-image selection functionality.
