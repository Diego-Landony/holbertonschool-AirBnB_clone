#!/usr/bin/python3
"""
Module that contains unittest for the console
"""

import unittest
from unittest.mock import patch
from io import StringIO
import pep8
import os
from console import HBNBCommand
from models.engine.file_storage import FileStorage
from models import storage


class TestConsoleCommands(unittest.TestCase):
    """Class for testing console commands"""

    @classmethod
    def setUpClass(cls):
        """Setup for the tests"""
        cls.console = HBNBCommand()
        cls.mock_stdout = StringIO()

    def tearDown(self):
        """Remove file (file.json) created as a result of testing"""
        FileStorage._FileStorage__objects = {}
        try:
            os.remove("file.json")
        except FileNotFoundError:
            pass

    def test_pep8_conformance_console(self):
        """Test that console.py conforms to PEP8."""
        pep8style = pep8.StyleGuide(quiet=True)
        result = pep8style.check_files(['console.py'])
        self.assertEqual(result.total_errors, 0, "Found code style errors.")

    def test_pep8_conformance_test_console(self):
        """Test that tests/test_console.py conforms to PEP8."""
        pep8style = pep8.StyleGuide(quiet=True)
        result = pep8style.check_files(['tests/test_console.py'])
        self.assertEqual(result.total_errors, 0, "Found code style errors.")

    def test_help_command(self):
        """Test the help command and check if text is correct"""
        expected_output = "Documented commands (type help <topic>):\n" \
                          "========================================\n" \
                          "Amenity    City  Place   State  all    create   help  show\n" \
                          "BaseModel  EOF   Review  User   count  destroy  quit  update"
        with patch('sys.stdout', new=self.mock_stdout):
            self.console.onecmd("help")
        self.assertIn(expected_output, self.mock_stdout.getvalue())

    def test_prompt(self):
        """Test if the console prompt is correct"""
        self.assertEqual("(hbnb) ", HBNBCommand.prompt)

    def test_create_command(self):
        """Test the create command"""
        with patch('sys.stdout', new=self.mock_stdout):
            self.console.onecmd("create BaseModel")
            output = self.mock_stdout.getvalue().strip()
            self.assertIn("BaseModel", output)
            self.assertTrue(len(output) > 0)

    def test_show_command(self):
        """Test the show command"""
        with patch('sys.stdout', new=self.mock_stdout):
            self.console.onecmd("create BaseModel")
            obj_id = self.mock_stdout.getvalue().strip()
            self.mock_stdout.truncate(0)  # Clear the mock output buffer
            self.console.onecmd(f"show BaseModel {obj_id}")
            output = self.mock_stdout.getvalue().strip()
            self.assertIn(obj_id, output)

    def test_show_missing_id(self):
        """Test the show command with missing instance id"""
        expected_output = "** instance id missing **"
        with patch('sys.stdout', new=self.mock_stdout):
            self.console.onecmd("show BaseModel")
            output = self.mock_stdout.getvalue().strip()
            self.assertEqual(expected_output, output)

    def test_show_nonexistent_instance(self):
        """Test the show command with a non-existent instance id"""
        expected_output = "** no instance found **"
        with patch('sys.stdout', new=self.mock_stdout):
            self.console.onecmd("show BaseModel 12345")
            output = self.mock_stdout.getvalue().strip()
            self.assertEqual(expected_output, output)

    def test_all_command(self):
        """Test the all command"""
        with patch('sys.stdout', new=self.mock_stdout):
            self.console.onecmd("create BaseModel")
            self.console.onecmd("create User")
            self.console.onecmd("create State")
            self.console.onecmd("create City")
            self.console.onecmd("create Amenity")
            self.console.onecmd("create Review")
            self.console.onecmd("create Place")
            self.mock_stdout.truncate(0)  # Clear the mock output buffer
            self.console.onecmd("all")
            output = self.mock_stdout.getvalue().strip()
            self.assertIn("BaseModel", output)
            self.assertIn("User", output)
            self.assertIn("State", output)
            self.assertIn("City", output)
            self.assertIn("Amenity", output)
            self.assertIn("Review", output)
            self.assertIn("Place", output)

    def test_count_command(self):
        """Test the count command"""
        with patch('sys.stdout', new=self.mock_stdout):
            self.console.onecmd("create BaseModel")
            self.console.onecmd("create BaseModel")
            self.mock_stdout.truncate(0)  # Clear the mock output buffer
            self.console.onecmd("BaseModel.count()")
            output = self.mock_stdout.getvalue().strip()
            self.assertEqual("2", output)

    def test_emptyline(self):
        """Test empty line"""
        with patch('sys.stdout', new=self.mock_stdout):
            self.console.onecmd("\n")
            output = self.mock_stdout.getvalue().strip()
            self.assertEqual('', output)

    def test_quit_command(self):
        """Test the quit command"""
        with patch('sys.stdout', new=self.mock_stdout):
            self.console.onecmd("quit")
            output = self.mock_stdout.getvalue().strip()
            self.assertEqual('', output)


if __name__ == "__main__":
    unittest.main()
