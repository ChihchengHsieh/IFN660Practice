# IFN660Practice
TakingNoteForIFN660



```c++

%union
{

  // Where we define the type for %type
  // These types are imported from AST.h
  Expression *expr;
	Statement *stmt;
	Type *type;
	vector<Statement*> *stmts;
	int num;
	char *name;

}










%token <num> NUMBER
%token <name> IDENT
%token IF ELSE INT BOOL

%type <expr> Expression
%type <stmt> Statement
%type <type> Type
%type <stmts> StatementList

<expr> is defined in %union{}

Expression -> the elements that we will read from the next section



%left '='
%nonassoc '<'
%left '+'




%%

Program : Statement											{ root = $1; }
        ;

Statement : IF '(' Expression ')' Statement ELSE Statement	
          | '{' StatementList '}'							
		  | Expression ';'									{ $$ = new ExpressionStatement($1); }
		  | Type IDENT ';'									
		  ;

Type : INT													
     | BOOL													
	 ;

StatementList : StatementList Statement				    	
              | /* empty */									
			  ;

Expression : NUMBER											{ $$ = new Literal($1);         }
           | IDENT											{ $$ = new IdentifierReference($1);     }
		   | Expression '=' Expression						{ $$ = new AssignmentExpression($1, $3); }
		   | Expression '+' Expression						
		   | Expression '<' Expression						
		   ;


%%




````
