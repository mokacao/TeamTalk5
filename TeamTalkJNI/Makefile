TEAMTALK_ROOT=$(PWD)/..
INCLUDE := -I$(TEAMTALK_ROOT)/TeamTalk_DLL
INCLUDE += -I/usr/lib/jvm/java-7-openjdk/include #Mac OS
INCLUDE += -I/usr/lib/jvm/java-7-openjdk-armhf/include #Raspberry
INCLUDE += -I/usr/lib/jvm/java-7-openjdk-i386/include #Debian i386
INCLUDE += -I/usr/lib/jvm/java-7-openjdk-amd64/include #Debian amd64
INCLUDE += -I/System/Library/Frameworks/JavaVM.framework/Versions/A/Headers #Mac OS
INCLUDE += -I/usr/lib/java/include # Slackware-14.1

TT5LIBS_DBG := -L$(TEAMTALK_ROOT)/TeamTalk_DLL -lTeamTalk5d
TT5PRODLIBS_DBG := -L$(TEAMTALK_ROOT)/TeamTalk_DLL -lTeamTalk5Prod

TT5DLIBS := -L$(TEAMTALK_ROOT)/TeamTalk_DLL -lTeamTalk5
TT5PRODLIBS := -L$(TEAMTALK_ROOT)/TeamTalk_DLL -lTeamTalk5Pro

#locations of 'hamcrest-core' and 'junit'
HAMCRESTCORE_JAR := hamcrest-core-1.3.jar
JUNIT_JAR := junit-4.11.jar

CPPFLAGS := -fPIC
ARCH=Linux

ifdef DYLD_LIBRARY_PATH
TT5DLL := $(PWD)/libs/libTeamTalk5-jni.dylib
TT5PRODLL := $(PWD)/libs/libTeamTalk5Pro-jni.dylib
else
TT5DLL := $(PWD)/libs/libTeamTalk5-jni.so
TT5PRODLL := $(PWD)/libs/libTeamTalk5Pro-jni.so
endif

help:
	@echo "Targets:"
	@echo -e "\tnative"
	@echo -e "\t\t" "Build JNI DLL for TeamTalk5 DLL"
	@echo -e "\tnativepro"
	@echo -e "\t\t" "Build JNI DLL for TeamTalk5Pro DLL"
	@echo -e "\tjava"
	@echo -e "\t\t" "Build TeamTalk5.jar. Jva wrapper classes for JNI DLLs"
	@echo -e "\tclean"
	@echo -e "\t\t" "Remove all intermediate files"
	@echo -e "\tcleanjava"
	@echo -e "\t\t" "Remove all Java generated classes and JAR files"
	@echo -e "\tjunit"
	@echo -e "\t\t" "Build and run JUnit test-cases"
	@echo -e "\ttestcase"
	@echo -e "\t\t" "Build and run a single test-case in TeamTalkTestCase-class"

native:
	cd jni && g++ $(INCLUDE) $(CPPFLAGS) $(TT5DLIBS) -shared teamtalk-jni.cpp ttconvert-jni.cpp -o $(TT5DLL) -Wno-unused-value

nativedbg:
	cd jni && g++ $(INCLUDE) $(CPPFLAGS) $(TT5DLIBS_DBG) -shared teamtalk-jni.cpp ttconvert-jni.cpp -o $(TT5DLL) -Wno-unused-value

nativepro:
	cd jni && g++ $(INCLUDE) $(CPPFLAGS) $(TT5PRODLIBS) -shared teamtalk-jni.cpp ttconvert-jni.cpp -o $(TT5PRODLL) -Wno-unused-value

nativeprodbg:
	cd jni && g++ $(INCLUDE) $(CPPFLAGS) $(TT5PRODLIBS_DBG) -shared teamtalk-jni.cpp ttconvert-jni.cpp -o $(TT5PRODLL) -Wno-unused-value

java:
	cd src && mkdir ./build && javac -g:vars -parameters -d ./build ./dk/bearware/*.java ./dk/bearware/events/*.java && cd ./build && jar cf ./TeamTalk5.jar ./dk/bearware/*.class ./dk/bearware/events/*.class

clean: cleanjava
	cd jni && rm $(TT5DLL) $(TT5PRODLL)

cleanjava:
	cd src && find . -name \*.class -exec rm {} \;
	cd test && find . -name \*.class -exec rm {} \;
	cd src && rm -r build

junitcompile: native
	cd test && javac -cp .:$(HAMCRESTCORE_JAR):$(JUNIT_JAR) -sourcepath ../src dk/bearware/TeamTalkTestCase.java dk/bearware/TeamTalkTestCaseBase.java TeamTalkTestSuite.java SingleJUnitTestRunner.java

junit:  junitcompile
	cd test && java -cp .:../src:$(HAMCRESTCORE_JAR):$(JUNIT_JAR) org.junit.runner.JUnitCore TeamTalkTestSuite

testcase: junitcompile
	cd test && java -cp .:../src:$(HAMCRESTCORE_JAR):$(JUNIT_JAR) SingleJUnitTestRunner dk.bearware.TeamTalkTestCase#$(TARGET)
