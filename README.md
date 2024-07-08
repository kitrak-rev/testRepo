def get_unique_from_file(input_file, output_file):
    # Read lines from input file
    with open(input_file, 'r') as f:
        lines = f.readlines()
    
    # Strip newline characters and create a set of unique lines
    unique_lines = set(line.strip() for line in lines)
    
    # Write unique lines to output file
    with open(output_file, 'w') as f:
        for line in unique_lines:
            f.write(line + '\n')

# Example usage:
input_file = 'input.txt'
output_file = 'output.txt'
get_unique_from_file(input_file, output_file)
