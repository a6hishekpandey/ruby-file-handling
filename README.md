# Learn what matters - Working with files in Ruby

## File Path

```ruby
File.join("", "my_dir", "test.rb")
#=> mac: /my_dir/test.rb
#=> windows: \my_dir\test.rb

# dev/example.rb
    puts "The file (relative): #{__FILE__}"                                  #=> The file (relative): example.rb
    puts "The file (absolute): #{File.expand_path(__FILE__)}"                #=> The file (absolute): /dev/example.rb 
    puts "The dir (relative): #{File.dirname(__FILE__)}"                     #=> The dir (relative): .
    puts "The dir (absolute): #{File.expand_path(File.dirname(__FILE__))}    #=> The dir (absolute): /dev

# notes:
#=> __dir__ is same as File.dirname(__FILE__)
```

## Work with Files

```ruby
# Access files
file = File.new(filepath, 'w')
# work with the file
file.close

File.open(filepath, 'w') do |file|
# work with file
end


# File Access Modes
#=> w: starting a new file
#=> r+: reading/writing to an existing file
#=> r: read-only access to a file
#=> a: appending data to the end of a file


# Write to files
file = File.new('groceries.txt', 'w')
file.puts "Grocery list"
file.print "Butter\n"
file.write "Milk\n"
file << "Sugar\n"
file.close


# Read from files
file = File.new('groceries.txt', 'r')
string1 = file.read(4)  #=> reads first 4 characters
string2 = file.read(4)  #=> reads next 4 characters

line1 = file.gets.chomp #=> reads first line
line2 = file.gets.chomp #=> reads next line


# eof (end of file)
file.gets  #=> nil
file.eof?  #=> true


# File pointer
file = File.new('groceries.txt', 'r')
file.pos       #=> 0
file.read(3)   #=> reads 3 characters
file.pos       #=> 3
file.pos += 3  #=> 6

file.rewind    #=> 0, means reset to beginning of the file
file.pos       #=> 0


# Read or write an entire file
File.read(filepath)       #=> returns string and does not remove '\n'
File.readlines(filepath)  #=> returns an array of strings and does not remove '\n'

File.write(filepath, string)


# Rename, delete, and copy
File.rename('oldname.txt', 'newname.txt')
File.delete('newname.txt')

require 'fileutils'
FileUtils.cp('orig.txt', 'dup.text')
FileUtils.mv('dup.txt', 'renamed.txt')  #=> renames the file
FileUtils.rm('renamed.txt')             #=> removes the file


# Examine file details
File.exists?(filepath)
File.file?(filepath)
File.directory?(filepath)
File.readable?(filepath)
File.writeable?(filepath)
File.executable?(filepath)
File.size(filepath)          #=> returns size in bytes
File.dirname(filepath)
File.basename(filepath)
File.extname(filepath)
File.mtime                   #=> returns last modified time (write)
File.atime                   #=> returns last accessed time (read or write)
File.ctime                   #=> returns status change time (read, write, permission)

# notes:
#=> filemode 'w' will wipe everything if file already exists
#=> << will always append at the end no matter where cursor was at that time
```

## Work with Directories

```ruby
# Create directories
Dir.mkdir(filepath)

require 'fileutils'
FileUtils.mkdir(filepath)

Dir.delete(filepath)       #=> does not delete unless empty

require 'fileutils'
FileUtils.rmdir(filepath)  #=> does not delete unless empty
FileUtils.rmdir_r(filepath)

Dir.empty?(filepath)


# Change directories
Dir.pwd    #=> returns present working directory
path = File.join('', 'User', 'Abhishek', 'Desktop')
Dir.chdir(path)


# Entries
array = Dir.entries(filepath)


# Glob
array = Dir.glob('*')
array = Dir.glob('report_201[5-9].{pdef,doc,docx}')

# Glob pattern symbols
#=> c*: matches files beginning with c
#=> *c: matches files ending with c
#=> *c*: matches all files with c in them
#=> **: matches directories recursively
#=> ?: matches any one char
#=> [a-z]: matches any one char in the set
#=> {p,q}: matches either literal p or literal q

# notes:
#=> both relative and absolute paths will work
#=> entries are contents of the directory (returns what we get while executing ls -la in a unix envirenment)
#=> glob returns an array of filenames which match a pattern and does not include entries for '.' and '..'
```

## Common Data Formats

```ruby
# CSV
#=> Read
require 'cvs'
CSV.read('data.csv').foreach do |row|
# use row here
end

array_of_arrays = CSV.read('data.csv')

#=> Write
require 'cvs'
CSV.open('data.csv', 'w') do |csv|
csv << ['row', 'of', 'CSV', 'data']
end

# CSV to Hashes
require 'csv'

headers = nil
presidens = []

CSV.foreach('us_presidents.csv') do |row|
    if headers == nil
        headers = row
    else
        presidens << row
    end
end

labels = headers.map {|item| item.downcase.gsub(/\s/, '_')}
new_hash = labels.zip(presidens.first).to_h


# YAML
#=> Read
require 'psych'
yaml = File.read('file.yml')
ruby_data = Psych.load(yaml)

#=> Write
require 'psych'
yaml = Psych.dump(ruby_data)
yaml = {'enabled' => true}.to_yaml
File.write('file.yaml', yaml)


# JSON
#=> Read
require 'json'
json = File.read('file.json')
hash = JSON.parse(json)

#=> Write
require 'json'
json = JSON.generate(hash)
json = {'enabled' => true}.to_json
File.write('file.json', json)
```
