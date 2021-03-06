[![Build Status](https://travis-ci.org/bobbylight/RSyntaxTextArea.svg?branch=master)](https://travis-ci.org/bobbylight/RSyntaxTextArea)
[![Coverage Status](https://coveralls.io/repos/bobbylight/RSyntaxTextArea/badge.svg)](https://coveralls.io/r/bobbylight/RSyntaxTextArea)

# INCLUDING BIRTH CERTIFICATE TO JAR
Idea is to include birth certificate data to jar file, generated from java project.
Syntax of the data is open but is may be for example json defining what checks and
validation is done to the java sources or binaries. For example:
 * Coverity static analysis scan
 * Protecode scan results.
 * Linters (findbugs,checkstyle)
 * JUNIT test coverage
 
The data is generated for example in CI system or in build phase.

# POC
In the small simple poc gradle build script of the existing open source java project
is touched to include json file to the generated archive and add reference to it 
to the manifest.

JUNIT test coverage obtained from jacoco analysis tool is used as birth certificate data.
Assumption is that build (and packing the jar) fails if JUNIT test cases fail, 
so passed/fail is not used as level of quality.

The commands that do the trick are
 * gradle jacocoTestReport; does the junit test case coverity analysis
  * Creates results (as megabytes of data) to build/reports/jacoco/test/jacocoTestReport.xml 
 * gradle doBirthCertificateAnalysis ; Reads the jacocoTestReport.xml and creates json out of it to file birthcertificate/birthcertificate.json 
 * gradle uploadArchives ; If the json file is found, includes the json file to the created archive and adds a reference to it to the manifest file. If the file does not exist a line 'birthCertificate:' is written.

The birth certificate json is like:
```
{"BirthCertificate": {
	"JUNIT": {
		"coverage": {"type":"jacoco"
			,"INSTRUCTION":{
				"missed":"66729"
				"covered":"38996"
				"ratio":"0.3688437"}
			,"BRANCH":{
				"missed":"9806"
				"covered":"3007"
				"ratio":"0.23468353"}
			,"LINE":{
				"missed":"16398"
				"covered":"9445"
				"ratio":"0.36547613"}
			,"COMPLEXITY":{
				"missed":"8702"
				"covered":"2883"
				"ratio":"0.24885628"}
			,"METHOD":{
				"missed":"1941"
				"covered":"1446"
				"ratio":"0.4269265"}
			,"CLASS":{
				"missed":"92"
				"covered":"206"
				"ratio":"0.6912752"}
			}
		}
	}
}
```  
Later:
 * The birth certificate gradle routines can be separeted to own gradle task class. 

# FROM THE ORIGINAL PROJECT
RSyntaxTextArea is a customizable, syntax highlighting text component for Java Swing applications.  Out of
the box, it supports syntax highlighting for 40+ programming languages, code folding, search and replace,
and has add-on libraries for code completion and spell checking.  Syntax highlighting for additional languages
[can be added](https://github.com/bobbylight/RSyntaxTextArea/wiki) via tools such as [JFlex](http://jflex.de).

RSyntaxTextArea is available under a [modified BSD license](https://github.com/bobbylight/RSyntaxTextArea/blob/master/src/main/dist/RSyntaxTextArea.License.txt).
For more information, visit [http://bobbylight.github.io/RSyntaxTextArea/](http://bobbylight.github.io/RSyntaxTextArea/).

Available in the [Maven Central repository](http://search.maven.org/#search%7Cga%7C1%7Crsyntaxtextarea%20jar) (`com.fifesoft:rsyntaxtextarea:XXX`).
SNAPSHOT builds of the in-development, unreleased version are hosted on [Sonatype](https://oss.sonatype.org/content/repositories/snapshots/com/fifesoft/rsyntaxtextarea/).

# Building

RSyntaxTextArea uses [Gradle](http://gradle.org/) to build.  To compile, run
all unit tests, and create the jar, run:

    ./gradlew build

Note that RSTA only requires Java 5.  To that end, the boot classpath will be set to accommodate
this if a variable `java5CompileBootClasspath` is set to the location of `rt.jar` in a Java 5 JDK.
This can be added to `<maven-home>/gradle.properties` if desired, to avoid diffs in the project's
`gradle.properties`.

When building with Java 8 or later, the `javadoc` task currently prints many warnings about Javadoc
"issues."  This is because
[doclint](http://blog.joda.org/2014/02/turning-off-doclint-in-jdk-8-javadoc.html)
is enabled by default in that release of Java.  These warnings are harmless, and in a future
release will be cleaned up.

# Example Usage

RSyntaxTextArea is simply a subclass of JTextComponent, so it can be dropped into any Swing application with ease.

```java
import javax.swing.*;
import org.fife.ui.rtextarea.*;
import org.fife.ui.rsyntaxtextarea.*;

public class TextEditorDemo extends JFrame {

   public TextEditorDemo() {

      JPanel cp = new JPanel(new BorderLayout());

      RSyntaxTextArea textArea = new RSyntaxTextArea(20, 60);
      textArea.setSyntaxEditingStyle(SyntaxConstants.SYNTAX_STYLE_JAVA);
      textArea.setCodeFoldingEnabled(true);
      RTextScrollPane sp = new RTextScrollPane(textArea);
      cp.add(sp);

      setContentPane(cp);
      setTitle("Text Editor Demo");
      setDefaultCloseOperation(EXIT_ON_CLOSE);
      pack();
      setLocationRelativeTo(null);

   }

   public static void main(String[] args) {
      // Start all Swing applications on the EDT.
      SwingUtilities.invokeLater(new Runnable() {
         public void run() {
            new TextEditorDemo().setVisible(true);
         }
      });
   }

}
```
# Sister Projects

RSyntaxTextArea provides syntax highlighting, code folding, and many other features out-of-the-box, but when building a code editor you often want to go further.  Below is a list of small add-on libraries that add more complex functionality:

* [AutoComplete](https://github.com/bobbylight/AutoComplete) - Adds code completion to RSyntaxTextArea (or any other JTextComponent).
* [RSTALanguageSupport](https://github.com/bobbylight/RSTALanguageSupport) - Code completion for RSTA for the following languages: Java, JavaScript, HTML, PHP, JSP, Perl, C, Unix Shell.  Built on both RSTA and AutoComplete.
* [SpellChecker](https://github.com/bobbylight/SpellChecker) - Adds squiggle-underline spell checking to RSyntaxTextArea.
* [RSTAUI](https://github.com/bobbylight/RSTAUI) - Common dialogs needed by text editing applications: Find, Replace, Go to Line, File Properties.

# Getting Help

* Add an issue on GitHub
* Peruse [the wiki](https://github.com/bobbylight/RSyntaxTextArea/wiki)
* Ask in the [project forum](http://fifesoft.com/forum/)
* Check the project's [home page](http://bobbylight.github.io/RSyntaxTextArea/)

