# Source and destination directories
source_dir="/path/to/source/"
destination_dir="/path/to/destination/"

# Array of files to copy
files=("file1.txt" "file2.txt" "file3.txt")

# Loop through each file
for file in "${files[@]}"
do
    # Check if the file exists in the destination
    if [ ! -f "${destination_dir}/${file}" ]; then
        echo "Copying ${file}..."
        cp "${source_dir}/${file}" "${destination_dir}"
        echo "${file} copied successfully."
    else
        echo "${file} already exists in the destination. Skipping copy."
    fi
done
