/////  # evasor-de-lineaa

#include "mbed.h"
#define T1 2
#define T2 3
#define A 0 
#define B 1 
#define C 2  
#define D 3 
#define E 4 
#define F 5  
#define G 6 
#define H 7 
#define I 8 
#define J 9  
 
int actual=A;

BusOut motor(PTB0, PTB1, PTB2, PTB3, PTE20, PTE21, PTE22, PTE23);

BusIn  entradas(PTC11,PTC10);


typedef struct {
uint8_t next[4];
uint16_t time;
uint8_t out;
}fsm_element;
    
    
fsm_element stepper[10]={
    {{B,E,H,D},T1,0x55},//S0, estado inicial
    {{C,C,C,A},T1,0x66},//S1, ambos hacia adelante
    {{D,D,D,B},T1,0xAA},//S2, ambos hacia adelante
    {{A,A,A,C},T1,0x99},//S3, ambos hacia adelante
    {{F,F,F,F},T2,0x69},//S4, der. adelante y izq atras, gira a la izquierda
    {{G,G,G,G},T2,0xAA},//S5, der. adelante y izq atras, gira a la izquierda
    {{A,A,A,A},T2,0x96},//S6, der. adelante y izq atras, gira a la izquierda
    {{I,I,I,I},T2,0x96},//S7, der. atras y izq adelante, gira a la derecha
    {{J,J,J,J},T2,0xAA},//S8, der. atras y izq adelante, gira a la derecha
    {{A,A,A,A},T2,0x69}//S9, der. atras y izq adelante, gira a la derecha
    
};

int main() {
    while(1) {
     motor = stepper[actual].out;
     wait_ms(stepper[actual].time);
     actual = stepper[actual].next[entradas];
    }
}
