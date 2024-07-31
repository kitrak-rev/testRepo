def is_file_empty_or_whitespace(file_path):
    try:
        with open(file_path, 'r') as file:
            content = file.read()
            # Check if the content is empty or contains only whitespace characters
            return not content or content.isspace()
    except FileNotFoundError:
        print(f"File not found: {file_path}")
        return False
    except IOError as e:
        print(f"IO error occurred: {e}")
        return False
