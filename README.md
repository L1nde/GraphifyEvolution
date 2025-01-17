# GraphifyEvolution
GraphifyEvolution is mainly built to analyse evolution of iOS applications in bulk, but it also supports other languages such as java and c++. The tool is built in a modular manner so that it could be easliy extended. Extensions for other languages, dependency managers, external analysers etc. are welcome (see [developer guide](documentation/developer_guide.md)).

GraphifyEvolution is an extension of the tool [GraphifySwift](https://github.com/kristiinara/GraphifySwift).

## Usage

### Prerequisites

GraphifyEvolution is written in swift and can currently only run on Mac OS. There are plans on trying to make the tool also work on linux. Currently there is no linux support as the Foundation framework is not yet fully implemented for linux. 

Neo4j database server needs to be running before GraphifyEvolution is run. Data url and authorization token for neo4j are currently hardcoded in DatabaseController and need to match the neo4j server configuration. 

Clone this repository. To build GraphifyEvolution run `swift build` in the project folder. When the build succeeds the GraphifyEvolution executable can be found in .build/debug.

### Running GraphifyEvolution

Analyse an application:
      
    GraphifyEvolution analyse <repository path>
    
additional options: 
	
     --app_key			<unique application key>		(optional argument)
     --evolution								(optional flag, analyse evolution of application, expects that application folder is a git repository)
     --bulk-json-path		<path>					(optional argument, clones and analyses applications listed in json file, applications are cloned to repository path)	
     --language			<swift/cpp/java>			(optional argument, specifies project language. Default value is swift.)
     --external-analysis		<duplication/insider/smells/metrics>	(optional argument, specifies external analyser to be used, can be used multiple times)
     

#### Examples: 

Evolution analysis with added code smell information

	GraphifyEvolution analyse <app folder> --external-analysis smells


Bulk analysis of java projects: 

	GraphifyEvolution analyse <apps folder> --bulk_json_path <json path> --language java

#### Analysing the application database

GraphifyEvolution enters structural information about the analysed applications and additional infromation gained from external analysers into a neo4j database. Cypher queries can be written and excecuted in the neo4j browser for data extraction and analysis. Some example queries are explained [here](documentation/example_queries.md).
    
## Database structure

More on the database structure [here](documentation/db_structure.md).

## Tool architecture

GraphifyEvolution consists of 8 main elements: 

* Main
* AppAnalysisController
* LocalFileManager (C++, Swift, and Java)
* AppManager (Simple, Git, and Bulk)
* DependencyManager (Simple, Maven, Gradle)
* SyntaxAnalyser (C++, Swift, and Java)
* ExternalAnalyser (Duplication, InsiderSec, Metrics, Smells)
* Database (neo4j)

Depending on user input implementations for [LocalFileManager](documentation/local_file_manager.md), [AppManager](documentation/documentation/app_manager.md), [DependencyManager](documentation/dependency_manager.md) and [SyntaxAnalyser](documentation/syntax_analyser.md) are chosen. New implementations for these protocols can be easily added, the requirements for each are described on the respective pages. 

AppAnalysisController is described [here](documentation/application_analysis_controller.md). 

## Known issues and future plans

- GraphifyEvolution is written in swift and can currently only run on Mac OS. There are plans on trying to make the tool also work on linux. Currently there is no linux support as the Foundation framework is not yet fully implemented for linux. 
- Additional dependency managers need to be implemented so that more applications could be analysed
  - Java projects can be anlysed if the project either contains all its dependencies or if it is using maven. Support for gradle projects is in the process of being added. 
  - Swift projects currently need all its dependencies ncluded in the project, support for cocoapods, swift package manager and potentially others will be added.
  - C++ projects need all its dependencies included in the project.
- A switch for verbose logging should be added
- Class implementations and method arguments are not yet handled by the syntax analyser
- Java projects that use lombock might not be analysed correctly. A delomboc script is added under JavaParser, but it does not always work and is very slow (it is currently not automatically called).
    
## Slack

Join us on Slack if you have any direct questions or would like to contribute.
[Invitation link.](https://join.slack.com/t/slack-xb79230/shared_invite/zt-rudaijhe-No387TJ33llqSTZa8da2PQ)
