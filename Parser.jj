/**
 * JavaCC template file created by SF JavaCC plugin 1.5.28+ wizard for JavaCC 1.5.0+
 */
options
{
  static = true;
}

PARSER_BEGIN(Compilador)
package edu.itsco;

import edu.itsco.semantica.*;
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import edu.itsco.generador.*;

public class Compilador
{

  public static AdministraVariables adminVariables;
  
  public static void main(String args [])
  throws ParseException, ExcepcionSemantica,
  FileNotFoundException
  {
    FileInputStream fis = new FileInputStream(
      	"entrada.txt");
    adminVariables = new AdministraVariables();
    Traductor.cabecera();
    System.out.println("Bienvenidos al compilador...");
    //Compilador c = new Compilador(System.in);
    Compilador c = new Compilador(fis);
    //llamamos la función principal
    c.principal();
    System.out.println("Compilo correctamente...");
    Traductor.pie();
  }
}

PARSER_END(Compilador)

SKIP :
{
  " "
| "\r"
| "\t"
| "\n"
}

TOKEN: {
< INICIO: "inicio" >
| < FIN: "fin" >

| < ENTERO: "entero" >
| < CADENA: "cadena" >
| < FLOTANTE: "flotante" >

| < VAR: "var" >

| < ESCRIBIR: "escribir" >
| < LEER: "leer" >

| < SI: "si" >
| < ENTONCES: "entonces" >
| < SINO: "sino" >

| < SELECCIONA: "selecciona" >
| < CASO: "caso" >
| < DEFECTO: "defecto" >
| < ROMPE: "rompe" >

| < PARA: "para" >
| < HASTA: "hasta" >
| < SIGUIENTE: "siguiente" >

| < HACER: "hacer" >
| < MIENTRAS: "mientras" >

}

TOKEN: {
< SUMA: "+" >
| < RESTA: "-" >
| < MULTI: "*" >
| < DIV: "/" >
| < MOD: "%" >

| < IGUAL: "=" >

| < MAYOR: ">" >
| < MENOR: "<" >

| < AND: "&&" >
| < OR: "||" >
| < NOT: "!" >

| < OPP: "(" >
| < OPC: ")" >

| < DP: ":" >
}

TOKEN: { //Generados por REGEX
< #digito: ["0" - "9"] >
| < #letra: ["a"-"z", "A"-"Z"] >

| < IDENTIFICADOR: ( < letra > | "_" ) (< letra > | < digito > | "_")* >
| < VALOR_ENTERO: (< digito >)+ >
| < VALOR_FLOTANTE: (< digito >)+ "." (< digito >)+ >
| < VALOR_CADENA: "\"" (~["\""])* "\"" >

}

void principal() throws ExcepcionSemantica: {
 //Codigo java
} {
  < INICIO > <IDENTIFICADOR >
	(
	  sentencias()
	)+
  < FIN >
}

void sentencias() throws ExcepcionSemantica: {}
{
	declararVariable()
| 	gramaticaEscribir()
| 	gramaticaLeer()
| 	gramaticaAsignacion()
|   gramaticaSi()
|   gramaticaSelecciona()
|   gramaticaPara()
|   gramaticaMientras()
|   gramaticaHacer ()
}
void declararVariable() throws ExcepcionSemantica: {
	Token id;
	Token tipo;
	boolean inicializada = false;
}
{
  < VAR > id = <IDENTIFICADOR > tipo = tipoDato()
  {
    Traductor.imprimeInstruccion("Dim ");
    Traductor.imprimeToken(id.image);
    Traductor.imprimeInstruccion(" as ");
    if(tipo.image.equals("cadena")) {
		Traductor.imprimeToken("String");
    }else if(tipo.image.equals("entero")) {
		Traductor.imprimeToken("Integer");
    }else {
		Traductor.imprimeToken("Float");
    }
  }
  [
  	{ Traductor.imprimeToken("="); }
    <IGUAL > valor() { inicializada = true; }
  ]
  {
    Variable v = new Variable();
    v.setNombre(id.image);
    v.setTipo(tipo.image);
    v.setInicializada(inicializada);
    adminVariables.addVariable(v);
    Traductor.saltoDeLinea();
  }
}

Token tipoDato(): {
	Token tipo;
}
{
  (
    tipo = < ENTERO >
    | tipo = < CADENA >
    | tipo = < FLOTANTE >
  )
  {
    return tipo;
  }
}

void valor() throws ExcepcionSemantica: {
	Token id;
	Token valor;
}
{
  (
    valor =  < VALOR_CADENA >
  | valor = < VALOR_ENTERO >
  | valor = < VALOR_FLOTANTE >
  |id = < IDENTIFICADOR > {
	 Variable v = new Variable();
	 v.setNombre(id.image);
	 adminVariables.existeVariable(v);
	 adminVariables.variableInicializada(v);
	 valor = id;
  }
  )
  {
    Traductor.imprimeToken(valor.image);
  }
}
void gramaticaEscribir() throws ExcepcionSemantica: { }
{
  {
    Traductor.imprimeInstruccion("Console.WriteLine(");
  }
  < ESCRIBIR > <OPP > valor()
  ( { Traductor.imprimeToken(" & "); }
  	< SUMA > valor() )*
  	< OPC >
  {
    Traductor.imprimeInstruccion(")");
    Traductor.saltoDeLinea();
  }
}
void gramaticaLeer()throws ExcepcionSemantica: {
	Token id;
}
{
  < LEER > <OPP > id = <IDENTIFICADOR > <OPC >
  {
	Variable v = new Variable();
	v.setNombre(id.image);
	adminVariables.existeVariable(v);
	v = adminVariables.getVariableByNombre(id.image);
	v.setInicializada(true);
	Traductor.imprimeLeer(v.getNombre(), v.getTipo());
  }
}
void gramaticaAsignacion()throws ExcepcionSemantica:{
	Token id;
}
{
   id = <IDENTIFICADOR> <IGUAL>
	{
	  Traductor.imprimeToken(id.image);
	  Traductor.imprimeToken("=");
	}
   operacionSimple()
   {
	Variable v = new Variable();
	v.setNombre(id.image);
	adminVariables.existeVariable(v);
	v = adminVariables.getVariableByNombre(id.image);
	v.setInicializada(true);
	Traductor.saltoDeLinea();
   }
}
void operacionSimple() throws ExcepcionSemantica: {}
{
   ( valor () | operacionParentesis() )
   ( operandoAritmetico() (valor () | operacionParentesis()) )*
}
void operacionParentesis() throws ExcepcionSemantica: {}
{
   < OPP > { Traductor.imprimeToken("(");}
   operacionSimple()
   <OPC> { Traductor.imprimeToken(")");}
}
void operandoAritmetico():{
	Token o;
}
{
   (
      o = <SUMA> | o = <RESTA> | o = <MULTI> | o = <DIV>
      | o = <MOD>
   )
   {
      Traductor.imprimeToken(o.image);
   }
}
void gramaticaSi() throws ExcepcionSemantica: {}
{
  < SI > condicion() < ENTONCES >
  	  ( sentencias())+
  	[<SINO > (sentencias()) + ]
  	< FIN > <SI >
}
void condicion() throws ExcepcionSemantica: { }
{
  condicionSimple() (opLogico() condicionSimple())*
}
void condicionSimple()throws ExcepcionSemantica: { }
{
  valor() opRelacional() valor()
}
void opLogico(): { }
{
  < AND > | < OR >
}
void opRelacional(): { }
{
  < MAYOR > [<IGUAL >] | < MENOR > [<IGUAL >] | < IGUAL > <IGUAL >| < NOT > <IGUAL >
}
void gramaticaSelecciona() throws ExcepcionSemantica:{
	Token id;
}
{ 
  <SELECCIONA> <OPP> id = <IDENTIFICADOR> <OPC >
  {
    Variable v =  new Variable();
    v.setNombre(id.image);
    adminVariables.existeVariable(v);
  }
  ( <CASO > <VALOR_ENTERO > <DP> sentencias() <ROMPE >)+
  [<DEFECTO > <DP > sentencias() <ROMPE >]
  <FIN > <SELECCIONA> 
}
void gramaticaPara() throws ExcepcionSemantica: {
	Token id;
}
{
   < PARA > id = <IDENTIFICADOR > <IGUAL > <VALOR_ENTERO >
   <HASTA > <VALOR_ENTERO >
   {
    Variable v =  new Variable();
    v.setNombre(id.image);
    adminVariables.existeVariable(v);
   }
   		( sentencias())+
   	< SIGUIENTE >
}
void gramaticaMientras() throws ExcepcionSemantica: { }
{
  < MIENTRAS > <OPP > condicion () < OPC >
  		( sentencias())+
  < FIN > <MIENTRAS >
}
void gramaticaHacer() throws ExcepcionSemantica: { }
{
  < HACER >
  	( sentencias() )+
  < FIN > < MIENTRAS > <OPP > condicion()< OPC >
}
