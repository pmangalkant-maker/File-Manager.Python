from pathlib import Path
import os


def createfile():
    try:
        name = input("Please tell me your file name :- ")
        path = Path(name)

        if not path.exists():
            with open(path, "w") as fs:
                data = input("What you want to write :- ")
                fs.write(data)

            print("File created successfully")

        else:
            print("File name already exists")

    except Exception as err:
        print(f"An error occurred as {err}")


def readfile():
    try:
        name = input("Please tell your file name :- ")
        path = Path(name)

        if path.exists():
            with open(path, "r") as fs:
                content = fs.read()
                print(f"Your file content is:\n{content}")

        else:
            print("Error: file does not exist")

    except Exception as err:
        print(f"An error occurred as {err}")


def updatefile():
    try:
        name = input("Please tell your file name :- ")
        path = Path(name)

        if path.exists():

            print("Operations")
            print("1. Renaming the file")
            print("2. Appending the content")
            print("3. Overwriting the file")

            choice = int(input("Enter your option :- "))

            if choice == 1:
                newname = input("Tell your new file name :- ")
                new_path = Path(newname)

                if not new_path.exists():
                    path.rename(new_path)
                    print("Renamed successfully")

                else:
                    print("File already exists")

            elif choice == 2:
                with open(path, "a") as fs:
                    data = input("What do you want to append :- ")
                    fs.write("\n" + data)

                print("Successfully appended")

            elif choice == 3:
                with open(path, "w") as fs:
                    data = input("What do you want to overwrite :- ")
                    fs.write(data)

                print("Successfully overwritten")

            else:
                print("Invalid choice")

        else:
            print("File does not exist")

    except Exception as err:
        print(f"An error occurred as {err}")


def deletefile():
    try:
        name = input("Please tell your file name :- ")
        path = Path(name)

        if path.exists():
            path.unlink()
            print("File deleted successfully")

        else:
            print("Error: no such file exists")

    except Exception as err:
        print(f"An error occurred as {err}")


print("Press 1 for creating a file")
print("Press 2 for reading a file")
print("Press 3 for updating a file")
print("Press 4 for deleting a file")

a = int(input("\nTell your response :- "))

if a == 1:
    createfile()

elif a == 2:
    readfile()

elif a == 3:
    updatefile()

elif a == 4:
    deletefile()

else:
    print("Invalid option")
