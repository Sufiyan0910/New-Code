# New-Code
//Bresenham
#include<stdio.h>
#include<stdlib.h>
#include<graphics.h>

int main() {
    int gd = DETECT, gm;
    

    int x1, x2, y1, y2, dy, dx, p;
    printf("Enter starting points (x1, y1) = ");
    scanf("%d%d", &x1, &y1);
    printf("Enter ending points (x2, y2) = ");
    scanf("%d%d", &x2, &y2);

    dy = abs(y2 - y1);
    dx = abs(x2 - x1);
    p = 2 * dy - dx;
initgraph(&gd, &gm,(char*)"");
    while (x1 != x2) {
        putpixel(x1, y1, WHITE);

        if (dy < dx) {
            if (p < 0) {
                x1++;
                p = p + 2 * dy;
            } else {
                x1++;
                y1++;
                p = p + 2 * dy - 2 * dx;
            }
        } else {
            if (p < 0) {
                y1++;
                p = p + 2 * dx;
            } else {
                x1++;
                y1++;
                p = p + 2 * dx - dy;
            }
        }
    }
getch();
    closegraph();
    
}


//DDA
#include <stdio.h>
#include <stdlib.h>
#include <graphics.h>

int main() {
    int gd = DETECT, gm;
    initgraph(&gd, &gm, ""); 

    float x1, x2, y1, y2, dy, dx;
    printf("Enter starting points (x1, y1) = ");
    scanf("%f%f", &x1, &y1);
    printf("Enter ending points (x2, y2) = ");
    scanf("%f%f", &x2, &y2);
    
    dy = abs(y2 - y1); 
    dx = abs(x2 - x1);

    while (x1 != x2) {
        putpixel(x1, y1, WHITE); 
        
        if (dx >= dy) {
            x1++;
            y1 += (dy / dx);
        } else {
            x1 += (dx / dy);
            y1++;
        }
    }

    getch(); 
    closegraph(); 
    return 0;
}

//boundary fill
#include <stdio.h>
#include <graphics.h>

void boundaryfill(int x, int y, int f_color, int b_color) {
    if (getpixel(x, y) != b_color && getpixel(x, y) != f_color) {
        putpixel(x, y, f_color);
        boundaryfill(x + 1, y, f_color, b_color);
        boundaryfill(x, y + 1, f_color, b_color);
        boundaryfill(x - 1, y, f_color, b_color);
        boundaryfill(x, y - 1, f_color, b_color);
    }
}

int main() {
    int gm, gd = DETECT, radius;
    int x, y;

    printf("Enter x and y positions for circle: ");
    scanf("%d %d", &x, &y);

    printf("Enter radius of circle: ");
    scanf("%d", &radius);

    initwindow(800, 600); // Use initwindow instead of initgraph

    circle(x, y, radius);
    boundaryfill(x, y, 4, 15);

    delay(5000);
    closegraph();

Â Â Â Â returnÂ 0;
}
///floodfill
#include <stdio.h>
#include <conio.h>
#include <graphics.h>

void flood(int x, int y, int fillColor, int boundaryColor);

int main() {
    int gd = DETECT, gm;
    initgraph(&gd, &gm, "C:\\Turboc3\\BGI");

    int x1, y1, x2, y2;

    printf("Enter the coordinates of the top-left corner (x1 y1): ");
    scanf("%d %d", &x1, &y1);

    printf("Enter the coordinates of the bottom-right corner (x2 y2): ");
    scanf("%d %d", &x2, &y2);

    rectangle(x1, y1, x2, y2);
    flood((x1 + x2) / 2, (y1 + y2) / 2, 10, getpixel(x1 + 5, y1 + 5)); // Start flood fill from inside the rectangle

    getch();
    closegraph();
    return 0;
}

void flood(int x, int y, int fillColor, int boundaryColor) {
    if (getpixel(x, y) == boundaryColor) {
        delay(1);
        putpixel(x, y, fillColor);
        flood(x + 1, y, fillColor, boundaryColor);
        flood(x - 1, y, fillColor, boundaryColor);
        flood(x, y + 1, fillColor, boundaryColor);
        flood(x, y - 1, fillColor, boundaryColor);
Â Â Â Â }
}

//midpoiny
#include <stdio.h>
#include <stdlib.h>
#include <graphics.h>
#include <math.h>

int main() {
    int x1, y1, r, d, x0, y0;
    int gd = DETECT, gm;

    printf("Enter x1: ");
    scanf("%d", &x1);
    printf("Enter y1: ");
    scanf("%d", &y1);
    printf("Enter radius (r): ");
    scanf("%d", &r);

    d = 3 - 2 * r;
    x0 = x1; // Set the center x-coordinate
    y0 = y1; // Set the center y-coordinate
    initgraph(&gd, &gm, NULL);

    x1 = 0;
    y1 = r;

    while (x1 <= y1) {
        putpixel(x1 + x0, y1 + y0, WHITE);
        putpixel(y1 + x0, x1 + y0, WHITE);
        putpixel(-x1 + x0, y1 + y0, WHITE);
        putpixel(-y1 + x0, x1 + y0, WHITE);
        putpixel(-x1 + x0, -y1 + y0, WHITE);
        putpixel(-y1 + x0, -x1 + y0, WHITE);
        putpixel(x1 + x0, -y1 + y0, WHITE);
        putpixel(y1 + x0, -x1 + y0, WHITE);

        if (d < 0) {
            d = d + 4 * x1 + 6;
        } else {
            d = d + 4 * (x1 - y1) + 10;
            y1--;
        }
        x1++;
    }

    getch();
    closegraph();

Â Â Â Â returnÂ 0;
}

//bezier
#include <graphics.h>
#include <math.h>
#include <conio.h>
#include <stdio.h>

// Function to draw a Bezier curve
void drawBezierCurve(int x[4], int y[4]) {
    double put_x, put_y, t;

    // Iterate through values of t from 0 to 1
    for (t = 0.0; t <= 1.0; t += 0.001) {
        // Calculate the x and y coordinates using the Bezier formula
        put_x = pow(1 - t, 3) * x[0] + 3 * t * pow(1 - t, 2) * x[1] + 3 * t * t * (1 - t) * x[2] + pow(t, 3) * x[3];
        put_y = pow(1 - t, 3) * y[0] + 3 * t * pow(1 - t, 2) * y[1] + 3 * t * t * (1 - t) * y[2] + pow(t, 3) * y[3];
        putpixel((int)put_x, (int)put_y, WHITE); // Plot the point on the curve
    }
}

int main() {
    int x[4], y[4], i;
    int gd = DETECT, gm;

    // Initialize the graphics system
    initgraph(&gd, &gm, "C:\\TURBOC3\\BGI");

    if (graphresult() != grOk) {
        printf("Graphics initialization failed. Exiting...\n");
        return 1;
    }

    printf("****** Bezier Curve ***********\n");
    printf("Please enter x and y coordinates:\n");

    // Input control points
    for (i = 0; i < 4; i++) {
        scanf("%d %d", &x[i], &y[i]);
        putpixel(x[i], y[i], 3); // Plot the control points
    }

    // Draw the Bezier curve
    drawBezierCurve(x, y);

    getch();
    closegraph(); // Close the graphics system
Â Â Â Â returnÂ 0;
}

//scaling
#include <stdio.h>
#include <conio.h>
#include <graphics.h>

int main() {
    int x, y, x1, y1, x2, y2;
    int gd = DETECT, gm;
    initgraph(&gd, &gm, "C:\\TDM-GCC-32\\BGI");

    printf("\t\t\t********** Scaling ***********\n");
    printf("\n\t\t\t Please enter first coordinate of Triangle = ");
    scanf("%d %d", &x, &y);
    printf("\n\t\t\t Please enter second coordinate of Triangle = ");
    scanf("%d %d", &x1, &y1);
    printf("\n\t\t\t Please enter third coordinate of Triangle = ");
    scanf("%d %d", &x2, &y2);
    
    // Set drawing color
    setcolor(RED);

    // Draw the original triangle
    line(x, y, x1, y1);
    line(x1, y1, x2, y2);
    line(x2, y2, x, y);

    // Take input for scaling factors
    int scl_fctr_x, scl_fctr_y;
    printf("\n\t\t\t Now Enter Scaling factor x and y: ");
    scanf("%d %d", &scl_fctr_x, &scl_fctr_y);

    // Apply scaling factors to the coordinates
    x = x * scl_fctr_x;
    x1 = x1 * scl_fctr_x;
    x2 = x2 * scl_fctr_x;
    y = y * scl_fctr_y;
    y1 = y1 * scl_fctr_y;
    y2 = y2 * scl_fctr_y;

    // Set drawing color to white
    setcolor(WHITE);

    // Draw the scaled triangle
    line(x, y, x1, y1);
    line(x1, y1, x2, y2);
    line(x2, y2, x, y);

    getch();
    closegraph();

Â Â Â Â returnÂ 0;
}

//translation
#include<conio.h>
#include<graphics.h>
#include<stdio.h>
int main()
{
int gd=DETECT,gm;
int x,y,x1,y1,x2,y2,tx,ty;
initgraph(&gd,&gm,"C:\TDM-GCC-32\bin");
printf("\n Please enter first coordinate of the triangle= ");
scanf("%d %d", &x,&y);
printf("\n Enter second coordinate of the trinagle = ");
scanf("%d %d",&x1,&y1);
printf("\n Enter third coordinate of the triangle = ");
scanf("%d %d",&x2,&y2);
printf("\n\t\t********** TRIANGLE before & after translation ***********");
line(x,y,x1,y1);
line(x1,y1,x2,y2);
line(x2,y2,x,y);
printf("\n Now enter the translation vector = ");
scanf("%d %d",&tx,&ty);
setcolor(RED);

line(x+tx,y+ty,x1+tx,y1+ty);
line(x1+tx,y1+ty,x2+tx,y2+ty);
line(x2+tx,y2+ty,x+tx,y+ty);
getch();
closegraph();
returnÂ 0;
}#include<conio.h>
#include<graphics.h>
#include<stdio.h>
int main()
{
int gd=DETECT,gm;
int x,y,x1,y1,x2,y2,tx,ty;
initgraph(&gd,&gm,"C:\TDM-GCC-32\bin");
printf("\n Please enter first coordinate of the triangle= ");
scanf("%d %d", &x,&y);
printf("\n Enter second coordinate of the trinagle = ");
scanf("%d %d",&x1,&y1);
printf("\n Enter third coordinate of the triangle = ");
scanf("%d %d",&x2,&y2);
printf("\n\t\t********** TRIANGLE before & after translation ***********");
line(x,y,x1,y1);
line(x1,y1,x2,y2);
line(x2,y2,x,y);
printf("\n Now enter the translation vector = ");
scanf("%d %d",&tx,&ty);
setcolor(RED);

line(x+tx,y+ty,x1+tx,y1+ty);
line(x1+tx,y1+ty,x2+tx,y2+ty);
line(x2+tx,y2+ty,x+tx,y+ty);
getch();
closegraph();
returnÂ 0;
}

//rotation
#include <stdio.h>
#include <graphics.h>
#include <math.h>

int main() {
    int gd = DETECT, gm;
    int x1, y1, x2, y2;
    double s, c, angle;
    int x1_new, y1_new, x2_new, y2_new;

    initgraph(&gd, &gm, "C:\\TDM-GCC-32\\BGI");

    setcolor(RED);
    printf("Enter coordinates of the line: ");
    scanf("%d %d %d %d", &x1, &y1, &x2, &y2);

    printf("Enter rotation angle (in degrees): ");
    scanf("%lf", &angle);
    
    // Store the original coordinates in new variables
    x1_new = x1;
    y1_new = y1;
    x2_new = x2;
    y2_new = y2;

    setbkcolor(WHITE);
    line(x1_new, y1_new, x2_new, y2_new);
    getch();

    setbkcolor(WHITE);
    c = cos(angle * 3.14159265 / 180);
    s = sin(angle * 3.14159265 / 180);

    // Calculate the new coordinates
    x1_new = floor(x1 * c + y1 * s);
    y1_new = floor(-x1 * s + y1 * c);
    x2_new = floor(x2 * c + y2 * s);
    y2_new = floor(-x2 * s + y2 * c);

    cleardevice();

    setcolor(RED);
    line(x1_new, y1_new, x2_new, y2_new);
    getch();
    closegraph();
    
Â Â Â Â returnÂ 0;
}

//shearing
#include<stdio.h>
#include<graphics.h>
#include<conio.h>
int main()
{
int gd=DETECT,gm;
int x,y,x1,y1,x2,y2,shear_f;
initgraph(&gd,&gm,"C:\TDM-GCC-32\bin");
printf("\n please enter first coordinate = ");
scanf("%d %d",&x,&y);
printf("\n please enter second coordinate = ");
scanf("%d %d",&x1,&y1);
printf("\n please enter third coordinate = ");
scanf("%d %d",&x2,&y2);
printf("\n please enter shearing factor x = ");
scanf("%d",&shear_f);
cleardevice();
line(x,y,x1,y1);
line(x1,y1,x2,y2);
line(x2,y2,x,y);
setcolor(YELLOW);
x=x+ y*shear_f;
x1=x1+ y1*shear_f;
x2=x2+ y2*shear_f;
line(x,y,x1,y1);
line(x1,y1,x2,y2);
line(x2,y2,x,y);
getch();
closegraph();
returnÂ 0;
}

//reflection xy
#include <stdio.h>
#include <conio.h>
#include <graphics.h>

int main() {
    int gd = DETECT, gm;
    initgraph(&gd, &gm, "C:\\Turboc3\\BGI"); // Change the path to match your environment

    int x, y; // Coordinates of the original point

    // Input the coordinates of the original point
    printf("Enter the X coordinate of the point: ");
    scanf("%d", &x);
    printf("Enter the Y coordinate of the point: ");
    scanf("%d", &y);

    // Draw the original point
    putpixel(x, y, WHITE);

    // Perform reflection along the X-axis
    int x_reflected = x;
    int y_reflected = -y;

    // Perform reflection along the Y-axis
    x_reflected = -x_reflected;
    
    // Draw the point after both reflections
    putpixel(x_reflected, y_reflected, WHITE); // Change the color to RED

    getch();
    closegraph();
    return 0;
}


//reflection y
2D REFLECTION Y AXIS
#include <stdio.h>
#include <graphics.h>

// Function to perform 2D reflection along the y-axis
void reflectY(int x, int y) {
    int new_x = -x;
    int new_y = y;

    // Draw the original point
    putpixel(x, y, RED);

    // Draw the reflected point
    putpixel(new_x, new_y, BLUE);

    delay(5000); // Display the points for 5 seconds
    closegraph();
}

int main() {
    int gd = DETECT, gm;
    initgraph(&gd, &gm, NULL);

    int x, y;

    printf("Enter the original point (x y): ");
    scanf("%d %d", &x, &y);

    reflectY(x, y);

Â Â Â Â returnÂ 0;
}

//reflection x

// C program for the above approach 
  
#include <conio.h> 
#include <graphics.h> 
#include <stdio.h> 
  
// Driver Code 
void main() 
{ 
    // Initialize the drivers 
    int gm, gd = DETECT, ax, x1 = 100; 
    int x2 = 100, x3 = 200, y1 = 100; 
    int y2 = 200, y3 = 100; 
  
    // Add in your BGI folder path 
    // like below initgraph(&gd, &gm, 
    // "C:\\TURBOC3\\BGI"); 
    initgraph(&gd, &gm, ""); 
    cleardevice(); 
  
    // Draw the graph 
    line(getmaxx() / 2, 0, getmaxx() / 2, 
         getmaxy()); 
    line(0, getmaxy() / 2, getmaxx(), 
         getmaxy() / 2); 
  
    // Object initially at 2nd quadrant 
    printf("Before Reflection Object"
           " in 2nd Quadrant"); 
  
    // Set the color 
    setcolor(14); 
    line(x1, y1, x2, y2); 
    line(x2, y2, x3, y3); 
    line(x3, y3, x1, y1); 
    getch(); 
  
    // After reflection 
    printf("\nAfter Reflection"); 
  
    // Reflection along origin i.e., 
    // in 4th quadrant 
    setcolor(4); 
    line(getmaxx() - x1, getmaxy() - y1, 
         getmaxx() - x2, getmaxy() - y2); 
  
    line(getmaxx() - x2, getmaxy() - y2, 
         getmaxx() - x3, getmaxy() - y3); 
  
    line(getmaxx() - x3, getmaxy() - y3, 
         getmaxx() - x1, getmaxy() - y1); 
  
    // Reflection along x-axis i.e., 
    // in 1st quadrant 
    setcolor(3); 
    line(getmaxx() - x1, y1, 
         getmaxx() - x2, y2); 
    line(getmaxx() - x2, y2, 
         getmaxx() - x3, y3); 
    line(getmaxx() - x3, y3, 
         getmaxx() - x1, y1); 
  
    // Reflection along y-axis i.e., 
    // in 3rd quadrant 
    setcolor(2); 
    line(x1, getmaxy() - y1, x2, 
         getmaxy() - y2); 
    line(x2, getmaxy() - y2, x3, 
         getmaxy() - y3); 
    line(x3, getmaxy() - y3, x1, 
         getmaxy() - y1); 
    getch(); 
  
    // Close the graphics 
    closegraph(); 
} 
