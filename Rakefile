<<<<<<< HEAD
task :create_remote_repo do
	ruby "create_repository.rb"
end

task :create_local_repo, [:master_repo_dir] do |t, args|

	if !(File.directory?("my_repositories/#{args[:master_repo_dir]}"))
		Dir.mkdir("my_repositories/#{args[:master_repo_dir]}")
	end

	Dir.chdir("my_repositories/#{args[:master_repo_dir]}") do
		puts `git init`
	end
end

# task :clone_remote_repo do
# 	exec("python cloner_with_repo_name.py")
# end

task :add_submodule, [:master_repo_dir, :sub_user, :sub_repo_name] do |t, args|
	Dir.chdir("my_repositories/#{args[:master_repo_dir].gsub!(/\A"|"\Z/, '')}") do
		`git submodule add https://github.com/#{args[:sub_user].gsub!(/\A"|"\Z/, '')}/#{args[:sub_repo_name].gsub!(/\A"|"\Z/, '')}`
	end
end

task :create_sub_repos, [:master_repo_dir, :github_user] do |t, args|
	Dir.foreach("my_repositories/#{args[:master_repo_dir]}") do |x|

		if File.directory?("my_repositories/#{args[:master_repo_dir]}/#{x}")
			if !(x == ".." || x == "." || x == ".git")
				 Dir.chdir("my_repositories/#{args[:master_repo_dir]}/#{x}") do

				 	puts `git init`
				 	puts `git add *`
				 	puts `git commit -m "Initial Commit"`
				 	puts `curl -u #{args[:github_user]} https://api.github.com/user/repos -d '{ "name": "#{x}" }'`
				 	puts `git remote add origin https://github.com/#{args[:github_user]}/#{x}.git`
				 	puts `git push origin master`

				 	Dir.chdir("..") do
				 		puts `git submodule add https://github.com/#{args[:github_user]}/#{x}.git`
				 	end
				 	
				 end
			end
		end
=======
require 'io/console'

task :submodulize_folder do
	puts "Please enter the name of the master folder"
	master_repo_dir = STDIN.gets
	puts "Please enter Github account name for master repository"
	main_github = STDIN.gets
	puts "Please enter Github account name for junk repositories"
	secondary_github = STDIN.gets
	puts "Please enter password for master GitHub account:"
	main_pass = STDIN.noecho(&:gets)
	puts "Please enter password for secondary (junk) GitHub account:"
	secondary_pass = STDIN.noecho(&:gets)

	master = {user: main_github.gsub("\n", ""), pass: main_pass.gsub("\n", "")}
	junk = {user: secondary_github.gsub("\n", ""), pass: secondary_pass.gsub("\n", "")}

	master_repo_dir = master_repo_dir.gsub("\n", "")
	#delete
	#`curl -u miketestgit01:passwd -X DELETE  https://api.github.com/repos/{miketestgit01}/{TestNew}`
	#post
	#`curl -u miketestgit01:passwd -X POST https://api.github.com/user/repos -d '{"name":"TestNew"}'`
	#workstation
	#post
	#puts `curl -u "#{junk_account[:user]}:#{junk_account[:pass]}" https://api.github.com/user/repos -d '{ "name": "#{folder.split('/')[-1]}" }'`
	
	
	Dir.mkdir("my_repositories/submodule_builder")

	Dir.chdir("my_repositories/#{master_repo_dir}") do |x|
		puts `git remote rm origin`
 		puts `git remote add origin https://#{master[:user]}:#{master[:pass]}@github.com/#{master[:user]}/#{x.split('/')[-1]}.git`
	end

	Dir.foreach("my_repositories/#{master_repo_dir}") do |x|
		if(File.directory?("my_repositories/#{master_repo_dir}/#{x}"))
			# Refactor possible
			if !(x == ".." || x == "." || x == ".git")
				 puts `mv my_repositories/#{master_repo_dir}/#{x} my_repositories/submodule_builder/#{x}`
				 initialize_submodule("my_repositories/submodule_builder/#{x}", junk)

				 Dir.chdir("my_repositories/#{master_repo_dir}") do |i|
				 	puts `git rm --cached -rf #{x}`
				 	puts `git submodule add https://github.com/#{junk[:user]}/#{x}`
				 	puts `git rm --cached -rf #{x}`
				 	puts `git add *`
				 	puts `git commit -m "Add submodule folder #{x}"`
				 	puts `git push origin master`
				 end
				 
			end
		end
	end

	puts `sudo rm -rf my_repositories/submodule_builder`



end



# def checkDir(folder)
# 	Dir.foreach(folder) do |x|
# 		checkDir(x)

# 	end
# end

def initialize_submodule(folder, junk_account)

	# folder is full path to folder e.g.(github_repo_submodulizer/my_repositories/test/folder)
	Dir.chdir("#{folder}") do |i|
		puts `git init`
		puts `git add *`
 		puts `git commit -m "Initial Commit"`

 		# creates empty repo using name of given folder as repo name.
 		# folder name is collected by spliting "folder" and string after last "/"
 		puts `curl -u "#{junk_account[:user]}:#{junk_account[:pass]}" https://api.github.com/user/repos -d '{ "name": "#{folder.split('/')[-1]}" }'`
 		puts `git remote rm origin`
 		#command hidden to hide credentials
 		 `git remote add origin https://#{junk_account[:user]}:#{junk_account[:pass]}@github.com/#{junk_account[:user]}/#{folder.split('/')[-1]}.git`
 		puts `git push origin master`

	end

	Dir.foreach(folder) do |x|
		# x is subfolder being operated on
		if(File.directory?("#{folder}/#{x}"))
			if !(x == ".." || x == "." || x == ".git")
				 initialize_submodule("#{folder}/#{x}", junk_account)

				 Dir.chdir("#{folder}") do |i|

				 	puts `git rm --cached -rf #{x}`
				 	puts `git submodule add https://github.com/#{junk_account[:user]}/#{x}`
				 	puts `git rm --cached -rf #{x}`
				 	puts `git add *`
				 	puts `git commit -m "Add submodule folder #{x}"`
				 	puts `git push origin master`
				 end


			end
		end
		
>>>>>>> 53bb2ce... Create task for converting all directories and subdirectories to submodules
	end
end
