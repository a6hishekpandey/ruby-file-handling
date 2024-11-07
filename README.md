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


# Examine file details
File.exists?(filepath)
File.file?(filepath)
File.directory?(filepath)
File.readable?(filepath)
File.writeable?(filepath)
File.executable?(filepath)

# notes:
#=> filemode 'w' will wipe everything if file already exists
#=> << will always append at the end no matter where cursor was at that time
```
