#!/usr/bin/python3
import getopt
import re
import sys


def info_and_error(info):
    print(info)
    print('Try "checkmail --help" for more information')
    sys.exit(2)


def parse_arguments(argv):
    file_name = ''
    separator = '|'
    email_field = 'email'
    try:
        opts, args = getopt.getopt(argv, 'hf:s:e:', ['help', 'file=', 'sep=', 'email='])
    
    except:
        info_and_error('Wrong sintax')
           
    for opt, arg in opts:
        if opt in ('-h', '--help'):
            usage()
            sys.exit(0)
        
        elif opt in ('-f', '--file'):
            file_name = arg
        
        if opt in ('-s', '--sep'):
            if len(arg) == 1:
                separator = arg
            else:
                info_and_error('Separator must have 1 character')
                
        if opt in ('-e', '--email'):
            email_field = arg
    
    if file_name == '':
        info_and_error('Missing file parameter')
    
    if len(separator) != 1:
        info_and_error('Separator must have 1 character')
    
    return file_name, separator, email_field
    

def usage():
    print('\nUsage:')
    print('    checkmail -f <filename> [OPTIONS]')
    print('Read a mailing text file called <filename> and create a new file called')
    print('EMAIL_SAFE_<filename> striping lines with wrong e-mail values.')
    print('\nOptions:')
    print('    -f, --file <filename>\tfile to proccess')
    print('    -s, --sep <separator>\tcharacter that separate columns (default: |)')
    print('    -e, --email <email column>\tcolumn name with email field (default: email)')
    print('    -h, --help\tdisplay this help and exit\n')
   

def process_list(filename, separator, email_field):
    pattern_dot = re.compile('\.+')
    pattern_at = re.compile('(\.?@\.?)')
    pattern_email = re.compile('[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Z|a-z]{2,}')
    
    with open(filename, 'r') as f:
        header = f.readline().strip()
        yield header
        cols = header.split(separator)
        email_index = cols.index(email_field)       
        for line in f:
            values = line.strip().split(separator)
            new_email = re.sub(pattern_dot, '.', values[email_index])
            new_email = re.sub(pattern_at, '@', new_email)
            if re.fullmatch(pattern_email, new_email):
                cols[email_index] = new_email
                yield '|'.join(values)
    

def process_file(filename, separator, email_field):
    with open(f'EMAIL_SAFE_{filename}', 'w') as f:
        email_list_generator = process_list(filename, separator, email_field)
        for line in email_list_generator:
            f.write(f'{line}\n')


if __name__ == '__main__':
    file_name, separator, email_field = parse_arguments(sys.argv[1:])
    process_file(file_name, separator, email_field)
