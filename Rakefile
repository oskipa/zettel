require 'byebug'

# tasks
task :zettel do
  "usage: rake zettel <topic>" if ARGV.length < 2
  argv_workaround
  topic = ARGV[1]

  zettel = ZettelCreator.new(topic)
  zettel.create()

  puts "new zettel: #{zettel.file_name}"
end

task :tags do
  tag_file = "./tags"
  File.delete(tag_file) if File.exist?(tag_file)

  File.open(tag_file, 'w') do |tags|
    Dir["./zettels/*"].each do |path|
      zettel = Zettel.new path
      tags.puts zettel.to_tag
    end
  end
end

task :index do
  index_file = "./index"
  File.delete(index_file) if File.exist?(index_file)

  index = ZettelIndex.new

  Dir["./zettels/*"].each do |path|
    zettel = Zettel.new path
    index <<  zettel
  end

  File.open(index_file, 'w') do |index_file|
    index.keys.each do |key|
      index_file.puts index.index_entry key  
    end
  end
end

task :update do
  Rake::Task[:tags].execute
  Rake::Task[:index].execute
end

task :help do
  ['zettel, creates new zettel card: "$rake zettel python" ', 
   'tags, creates c-tags for hyperlink navigation in vim: "$rake tags"', 
   'index, updates index file: "$rake index"', 
   'update, runs tags and index together: "$rake update"',
   'help, shows help: "$rake help"'
  ].sort.each do |task|
    puts "#{task}"
  end
end

# support
def argv_workaround()
  ARGV.each { |a| task a.to_sym do ; end }
end


# classes
class Zettel
  def initialize(path=nil)
    @metadata = {}
    @content = ""
    @text = ""
    @path = ''
    load(path) unless path.nil?
  end

  def load(path)
    @path = path

    File.open(path, 'r') do |file|
        @text = file.read 
        meta, @content = @text.split '---'
        @metadata = meta_to_hash meta 
    end
  end

  def meta_to_hash(text)
    result = {}
    text.split("\n").each do |line|
      key, value = line.split(':')
      result[key.strip] = value.strip
    end

    result
  end

  def to_tag()
    "#{@metadata['id']}\t#{@path}\t/#{@metadata['id']}/"
  end

  def id()
    @metadata['id']
  end

  def keywords()
    @metadata["keywords"].strip.split(',').map {|k| k.strip}
  end

  def to_link()
    "[#{id}](#{@path})"
  end

  def <=>(other)
    id <=> other.id 
  end
  
end

class ZettelIndex
  def initialize()
    @index = {}
  end

  def <<(item)
    item.keywords.each do |key|
      @index[key] = [] unless @index.keys.include?(key)
      @index[key] << item
    end
  end

  def keys()
    @index.keys.sort
  end

  def index_entry(key)
    links = @index[key].sort.map{|zettel| zettel.id }.join(", ") 
    "#{key}: #{links}"
  end
end

class ZettelCreator
  def initialize(topic)
    @directory = "zettels"
    @topic = topic
    @id = create_id
  end

  def file_name()
    "#{@directory}/#{@id}_#{@topic}.md"
  end

  def create_id()
    now = Time.now
    now.strftime "%Y%m%d%H%M"
  end

  def create()
    File.open(file_name, 'w') do |file|
      file.write template
    end
  end

  def template()
  result = <<~ZETTEL
    topic: #{@topic}
    id: #{@id}
    keywords:
    ---

    # #{@id} #{@topic}



    # References
  ZETTEL
  end
end

