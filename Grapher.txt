#Program to plot, animate different kinds of 2D/3D graphs.
#Written at University of Toronto by Foster, Teng for CSCA20
#Input: mathematical function, variables ranges
#Output: 2D plot, 3D plot
#Explanation: 1.inverse sin(x)            = arcsin(x)
#             2.|x|                       = absolute(x)
#             3.Product sign              = *
#             4.Power sign                = ** or ^
#             5.All logarithmic in base e = ln(x)
#             6.No arcsec(x), arccsc(x), arccot(x) available
#Reference:
""" 
Title: Matplotlib Documentation
Author: John Hunter, Darren Dale, Eric Firing, Michael Droettboom and the Matplotlib development team
Date: 2002–2022
Code version: 3.6.2
Availability: https://matplotlib.org/stable/index.html 
"""
import matplotlib.pyplot as plt
from mpl_toolkits import mplot3d
import numpy as np
import matplotlib.animation as animation

xyzt_dic = {
    'xmin' : None,
    'xmax' : None,
    'ymin' : None,
    'ymax' : None,
    'zmin' : None,
    'zmax' : None,
    'tmin' : None,
    'tmax' : None,
    'rmin' : None,
    'rmax' : None, 
    'ϴmin' : None,
    'ϴmax' : None    
}

keyword_dic = {
    "^" : "**",
    "e" : "np.e", 
    "sin" : "np.sin", 
    "cos" : "np.cos",
    "tan" : "np.tan",
    "sEc" : "1 / np.cos",
    "csc" : "1 / np.sin",
    "cot" : "1 / np.tan",
    "arcSIN" : "np.arcsin",
    "arcCOS" : "np.arccos",
    "arcTAN" : "np.arctan",
    "pi" : "np.pi",
    "ln" : "np.log",
    "absolutE" : "np.absolute",
    "sqrt" : "np.sqrt",
}

keyword_revision_dic = {
    "arcsin" : "arcSIN", 
    "arccos" : "arcCOS",
    "arctan" : "arcTAN", 
    "absolute" : "absolutE",
    "sec" : "sEc"
}

#change the keyword to the ones that the computer accepts 
def keyword_revision(function_string):
    for key in keyword_dic:
        if key in function_string:  
            function_string = function_string.replace(key, keyword_dic.get(key))            
    return function_string

#change arc trigonometric function format
def keyword_pre_revision(function_string):
    for key in keyword_revision_dic:
        if key in function_string:
            function_string = function_string.replace(key, keyword_revision_dic.get(key))
    return function_string

#ask for input of x, y, z, t ranges 
def xyzt_range_input(xyzt_list):
    if 'x' in xyzt_list:
        xyzt_dic['xmin'] = float(input("Please enter the minimum x: "))
        xyzt_dic['xmax'] = float(input("Please enter the maximum x: ")) 
    if 'y' in xyzt_list:
        xyzt_dic['ymin'] = float(input("Please enter the minimum y: "))
        xyzt_dic['ymax'] = float(input("Please enter the maximum y: "))
    if 'z' in xyzt_list:
        xyzt_dic['zmin'] = float(input("Please enter the minimum z: "))
        xyzt_dic['zmax'] = float(input("Please enter the maximum z: "))
    if 't' in xyzt_list:
        xyzt_dic['tmin'] = float(input("Please enter the minimum t: "))
        xyzt_dic['tmax'] = float(input("Please enter the maximum t: "))
    if 'r' in xyzt_list:
        xyzt_dic['rmin'] = float(input("Please enter the minimum r: "))
        xyzt_dic['rmax'] = float(input("Please enter the maximum r: "))  
    if 'ϴ' in xyzt_list:
        xyzt_dic['ϴmin'] = float(input("Please enter the minimum ϴ: "))
        xyzt_dic['ϴmax'] = float(input("Please enter the maximum ϴ: "))    
            
def _2D_explicit_function_():
    print("Example: y(x) = e^(-x)*sin(10*x)")
    print("         xmin = 0, xmax = 10") 
    function = input("Please enter the function: y(x) = ")
    function = keyword_pre_revision(function)
    function = keyword_revision(function)
    code_string = "y = " + function
    global xyzt_dic
    xyzt_range_input(['x', ])
    function_list = [code_string, xyzt_dic.get('xmin'), xyzt_dic.get('xmax')]
    xyzt_dic = {key : None for key in xyzt_dic}
    return function_list 

def _2D_implicit_function_():
    print("Example: f(x, y) = x^2 + y^2 - 1")
    print("         xmin = -2, xmax = 2, ymin = -2, ymax = 2")
    function = input("Please enter the function: f(x, y) = ")
    function = keyword_pre_revision(function)
    function = keyword_revision(function)
    code_string = "z = " + function
    global xyzt_dic
    xyzt_range_input(['x', 'y'])
    function_list = [code_string, xyzt_dic.get('xmin'), xyzt_dic.get('xmax'), xyzt_dic.get('ymin'), xyzt_dic.get('ymax')]
    xyzt_dic = {key : None for key in xyzt_dic} 
    return function_list   

def _2D_explicit_polar_function_():
    print("Example: r(ϴ) = cos(ϴ)*sin(ϴ)")
    print("         ϴmin = 0, ϴmax = 6.28")
    function = input("Please enter the function: r(ϴ) = ")
    function = keyword_pre_revision(function)
    function = keyword_revision(function)
    code_string = "r = " + function
    global xyzt_dic
    xyzt_range_input(['ϴ', ])
    function_list = [code_string, xyzt_dic.get('ϴmin'), xyzt_dic.get('ϴmax')]
    xyzt_dic = {key : None for key in xyzt_dic}    
    return function_list

def _2D_implicit_polar_function_():
    print("Example: f(r, ϴ) = r^2 - sin(2*ϴ)")
    print("         rmin = 0, rmax = 1, ϴmin = 0, ϴmax = 6.28")
    function = input("Please enter the function: f(r, ϴ) = ")
    function = keyword_pre_revision(function)
    function = keyword_revision(function)
    code_string = "z = " + function
    global xyzt_dic
    xyzt_range_input(['r', 'ϴ'])
    function_list = [code_string, xyzt_dic.get('rmin'), xyzt_dic.get('rmax'), xyzt_dic.get('ϴmin'), xyzt_dic.get('ϴmax')]
    xyzt_dic = {key : None for key in xyzt_dic}
    return function_list     

def _2D_polar_parametric_function_():
    print("Example: x(r, ϴ) = 16*sin(ϴ)^3")
    print("         y(r, ϴ) = 13*cos(ϴ) - 5*cos(2*ϴ) - 2*cos(3*ϴ) - cos(4*ϴ)")
    print("         rmin = 0, rmax = 1, ϴmin = 0, ϴmax = 6.28")
    function1 = input("Please enter the function: x(r, ϴ) = ")
    function2 = input("Please enter the function: y(r, ϴ) = ")
    function1 = keyword_pre_revision(function1)
    function2 = keyword_pre_revision(function2)
    function1 = keyword_revision(function1)
    function2 = keyword_revision(function2)
    code_string1 = "x = " + function1
    code_string2 = "y = " + function2
    global xyzt_dic
    xyzt_range_input(['r', 'ϴ'])
    function_list = [code_string1, code_string2, xyzt_dic.get('rmin'), xyzt_dic.get('rmax'), xyzt_dic.get('ϴmin'), xyzt_dic.get('ϴmax')]
    xyzt_dic = {key : None for key in xyzt_dic}    
    return function_list      

def _2D_parametric_function_():
    print("Example: x(t) = t*sin(t)")
    print("         y(t) = e^t")
    print("tmin = 0, tmax = 1")
    function1 = input("Please enter the function: x(t) = ")
    function2 = input("Please enter the function: y(t) = ")
    function1 = keyword_pre_revision(function1)
    function2 = keyword_pre_revision(function2)
    function1 = keyword_revision(function1)
    function2 = keyword_revision(function2)
    code_string1 = "x = " + function1
    code_string2 = "y = " + function2
    global xyzt_dic
    xyzt_range_input(['t', ])
    function_list = [code_string1, code_string2, xyzt_dic.get('tmin'), xyzt_dic.get('tmax')]
    xyzt_dic = {key : None for key in xyzt_dic}    
    return function_list     

def _2D_vector_field_():
    print("Example: dy/dx = x^2 + y^2")
    print("         xmin = -1, xmax = 1, ymin = -1, ymax = 1")
    function = input("Please enter the function: dy/dx = ")
    function = keyword_pre_revision(function)
    function = keyword_revision(function)
    code_string = "z = " + function
    global xyzt_dic
    xyzt_range_input(['x', 'y'])
    function_list = [code_string, xyzt_dic.get('xmin'), xyzt_dic.get('xmax'), xyzt_dic.get('ymin'), xyzt_dic.get('ymax')]
    xyzt_dic = {key : None for key in xyzt_dic}     
    return function_list     

def _2D_contours_():
    print("Example: z(x, y) = sin(2*x)*cos(y)")
    print("         xmin = 0, xmax = 6.28, ymin = 0, ymax = 6.28")
    function = input("Please enter the function: z(x, y) = ")
    function = keyword_pre_revision(function)
    function = keyword_revision(function)
    code_string = "z = " + function
    global xyzt_dic
    xyzt_range_input(['x', 'y'])
    function_list = [code_string, xyzt_dic.get('xmin'), xyzt_dic.get('xmax'), xyzt_dic.get('ymin'), xyzt_dic.get('ymax')]
    xyzt_dic = {key : None for key in xyzt_dic} 
    return function_list     

def errorbar():
    print("Example: y(x) = exp(-x)")
    print("         xmin = 0, xmax = 1")
    print("         xerr = 0.1, yerr = 0.2")
    function = input("Please enter the function: y(x) = ")
    function = keyword_pre_revision(function)
    function = keyword_revision(function)    
    code_string = "y = " + function
    global xyzt_dic
    xyzt_range_input(['x', ])
    xerr = float(input("Please input x error: "))
    yerr = float(input("Please input y error: "))
    function_list = [code_string, xyzt_dic.get('xmin'), xyzt_dic.get('xmax'), xerr, yerr]
    xyzt_dic = {key : None for key in xyzt_dic}
    return function_list 

def vague_contours():
    print("Example: z(x, y) = 2*(e^(- x^2 - y^2) - e^(- (x-1)^2 - (y-1)^2))")
    print("         xmin = -3, xmax = 3, ymin = -3, ymax = 3")
    function = input("Please enter the function: z(x, y) = ")
    function = keyword_pre_revision(function)
    function = keyword_revision(function)
    code_string = "z = " + function
    global xyzt_dic
    xyzt_range_input(['x', 'y'])
    function_list = [code_string, xyzt_dic.get('xmin'), xyzt_dic.get('xmax'), xyzt_dic.get('ymin'), xyzt_dic.get('ymax')]
    xyzt_dic = {key : None for key in xyzt_dic} 
    return function_list 

def _animate_2D_function_():
    print("Example: y(x, t) = sin(2*x)*cos(t)")
    print("         xmin = 0, xmax = 6.28, ymin = -1, ymax = 1")
    function = input("Please enter the function: y(x, t) = ")
    function = keyword_pre_revision(function)
    function = keyword_revision(function)    
    global xyzt_dic
    xyzt_range_input(['x', 'y'])
    function_list = [function, xyzt_dic.get('xmin'), xyzt_dic.get('xmax'), xyzt_dic.get('ymin'), xyzt_dic.get('ymax')]
    xyzt_dic = {key : None for key in xyzt_dic} 
    return function_list    

def _3D_surface_function_():
    print("Example: z(x, y) = x^2 - cos(y)")
    print("         xmin = -1, xmax = 1, ymin = -1, ymax = 1")
    function = input("Please enter the function: z(x, y) = ")
    function = keyword_pre_revision(function)
    function = keyword_revision(function)
    code_string = "z = " + function
    global xyzt_dic
    xyzt_range_input(['x', 'y'])
    function_list = [code_string, xyzt_dic.get('xmin'), xyzt_dic.get('xmax'), xyzt_dic.get('ymin'), xyzt_dic.get('ymax')]
    xyzt_dic = {key : None for key in xyzt_dic} 
    return function_list   

def _3D_parametric_function_():
    print("Example: x(t) = sin(t)")
    print("         y(t) = cos(t)")
    print("         tmin = 0, tmax = 50")
    function1 = input("Please enter the function: x(t) = ")
    function2 = input("Please enter the function: y(t) = ")
    function1 = keyword_pre_revision(function1)
    function2 = keyword_pre_revision(function2)
    function1 = keyword_revision(function1)
    function2 = keyword_revision(function2)
    code_string1 = "x = " + function1
    code_string2 = "y = " + function2
    global xyzt_dic
    xyzt_range_input(['t', ])
    function_list = [code_string1, code_string2, xyzt_dic.get('tmin'), xyzt_dic.get('tmax')]
    xyzt_dic = {key : None for key in xyzt_dic} 
    return function_list     

def _3D_t_vector_field_():
    print("Example: x(t) = sin(t)")
    print("         y(t) = cos(t)")
    print("         z(t) = t")
    print("         tmin = 0, tmax = 10")
    function1 = input("Please enter the function: x(t) = ")
    function2 = input("Please enter the function: y(t) = ")
    function3 = input("Please enter the function: z(t) = ")
    function1 = keyword_pre_revision(function1)
    function2 = keyword_pre_revision(function2)
    function3 = keyword_pre_revision(function3)
    function1 = keyword_revision(function1)
    function2 = keyword_revision(function2)
    function3 = keyword_revision(function3)
    code_string1 = "x = " + function1
    code_string2 = "y = " + function2    
    code_string3 = "z = " + function3  
    global xyzt_dic
    xyzt_range_input(['t', ])
    function_list = [code_string1, code_string2, code_string3, xyzt_dic.get('tmin'), xyzt_dic.get('tmax')]
    xyzt_dic = {key : None for key in xyzt_dic} 
    return function_list    

def _3D_xyz_vector_field_():
    print("Example: v1(x, y, z) = -y")
    print("         v2(x, y, z) = -z")
    print("         v3(x, y, z) = x")
    print("         xmin = -10, xmax = 10, ymin = -10, ymax = 10, zmin = -10, zmax = 10")
    function1 = input("Please enter the function: v1(x, y, z) = ")
    function2 = input("Please enter the function: v2(x, y, z) = ")
    function3 = input("Please enter the function: v3(x, y, z) = ")
    function1 = keyword_pre_revision(function1)
    function2 = keyword_pre_revision(function2)
    function3 = keyword_pre_revision(function3)
    function1 = keyword_revision(function1)
    function2 = keyword_revision(function2)
    function3 = keyword_revision(function3)
    code_string1 = "v1 = " + function1
    code_string2 = "v2 = " + function2    
    code_string3 = "v3 = " + function3  
    global xyzt_dic
    xyzt_range_input(['x', 'y', 'z'])
    function_list = [code_string1, code_string2, code_string3, xyzt_dic.get('xmin'), xyzt_dic.get('xmax'), xyzt_dic.get('ymin'), xyzt_dic.get('ymax'), xyzt_dic.get('zmin'), xyzt_dic.get('zmax')]
    xyzt_dic = {key : None for key in xyzt_dic} 
    return function_list

def _3D_surface_animation_():
    print("Example: z(x, y, t) = sin(x - 0.2*t) * cos(y - 0.2*t)")
    print("         xmin = 0, xmax = 6.28, ymin = 0, ymax = 6.28")
    print("         t = 0 to t = 100")
    function = input("Please enter the function: z(x, y, t) = ")
    function = keyword_pre_revision(function)
    function = keyword_revision(function)    
    global xyzt_dic
    xyzt_range_input(['x', 'y'])
    frames = int(input("Animation starts from t = 0 to t = ?: "))   #needs to say t = integer
    function_list = [function, xyzt_dic.get('xmin'), xyzt_dic.get('xmax'), xyzt_dic.get('ymin'), xyzt_dic.get('ymax'), frames]
    xyzt_dic = {key : None for key in xyzt_dic} 
    return function_list    
    
def show_2D(ax):
    ax.grid()
    ax.set_xlabel('x')
    ax.set_ylabel('y')        
    plt.show() 

def show_3D(ax):
    ax.grid()
    ax.set_xlabel('x')
    ax.set_ylabel('y')
    ax.set_zlabel('z')
    plt.show()   
    
menu_options = 'A'
    
while menu_options != 'Z':

    menu_options = """
    plot 2D:
        A = plot y = f(x) explicitly                       
        B = plot f(x, y) = 0 implicitly             
        C = plot r = f(ϴ) on x-y axis explicitly    
        D = plot f(r, ϴ) = 0 on x-y axis implicitly 
        E = plot y(r, ϴ) = f(x(r, ϴ))               
        F = plot y(t) = f(x(t))                        
        G = plot vector field                       
        H = plot contours                           
        I = plot errorbar                          
        J = plot vague contours                  
        K = animate y = f(x, t)                    
    plot 3D:
        L = plot surface z = f(x, y)               
        M = plot f(x, y, z) = f(x(t), y(t), t)       
        N = plot vector field v = (x(t), y(t), z(t))
        O = plot vector field v = (v1(x, y, z), v2(x, y, z), v3(x, y, z))      
        P = animate surface z = f(x, y, t)         
        Z = End the program                                   
    Choose an option: 
    """
    
    function_type = input(menu_options)
    
    if function_type == 'A':
        f_list = _2D_explicit_function_()
        x = np.linspace(f_list[1], f_list[2], 1000)    
        exec(f_list[0]) #Run the code from the input function
        ax = plt.axes()
        ax.plot(x, y)
        show_2D(ax)     
    elif function_type == 'B':
        f_list = _2D_implicit_function_()
        xrange = np.linspace(f_list[1], f_list[2], 500)  
        yrange = np.linspace(f_list[3], f_list[4], 500)   
        x, y = np.meshgrid(xrange, yrange)
        exec(f_list[0]) #Run the code from the input function
        ax = plt.axes()
        ax.contour(x, y, z, [0]) #[0] means setting z(x, y) to zero
        show_2D(ax)         
    elif function_type == 'C':
        f_list = _2D_explicit_polar_function_()
        ϴ = np.linspace(f_list[1], f_list[2], 1000)    
        exec(f_list[0]) #Run the code from the input function
        x = r * np.cos(ϴ)
        y = r * np.sin(ϴ)
        ax = plt.axes()
        ax.plot(x, y)
        show_2D(ax) 
    elif function_type == 'D':
        f_list = _2D_implicit_polar_function_()
        rrange = np.linspace(f_list[1], f_list[2], 500)
        ϴrange = np.linspace(0, 2*np.pi, 500)    
        r, ϴ = np.meshgrid(rrange, ϴrange)
        exec(f_list[0]) #Run the code from the input function
        x, y = r * np.cos(ϴ), r * np.sin(ϴ)
        ax = plt.axes()
        ax.contour(x, y, z, [0]) #[0] means setting z(x, y) to zero
        show_2D(ax)   
    elif function_type == 'E':
        f_list = _2D_polar_parametric_function_()
        r = np.linspace(f_list[2], f_list[3], 500)  
        ϴ = np.linspace(f_list[4], f_list[5], 500)   
        exec(f_list[0]) #Run the code from the input function
        exec(f_list[1]) #Run the code from the input function
        ax = plt.axes()
        ax.plot(x, y)
        show_2D(ax)        
    elif function_type == 'F':
        f_list = _2D_parametric_function_()
        t = np.linspace(f_list[2], f_list[3], 1000)    
        exec(f_list[0]) #Run the code from the input function
        exec(f_list[1]) #Run the code from the input function
        ax = plt.axes()
        ax.plot(x, y)
        show_2D(ax)           
    elif function_type == 'G':
        f_list = _2D_vector_field_()
        xrange = np.linspace(f_list[1], f_list[2], 30)  
        yrange = np.linspace(f_list[3], f_list[4], 30)   
        x, y = np.meshgrid(xrange, yrange)
        exec(f_list[0]) #Run the code from the input function
        dx, dy = np.gradient(z)
        ax = plt.axes()
        ax.quiver(x, y, dx, dy)
        ax.set(aspect = 1)
        show_2D(ax)     
    elif function_type == 'H':
        f_list = _2D_contours_()
        xrange = np.linspace(f_list[1], f_list[2], 500)  
        yrange = np.linspace(f_list[3], f_list[4], 500)   
        x, y = np.meshgrid(xrange, yrange)
        exec(f_list[0]) #Run the code from the input function
        ax = plt.axes()
        cs = ax.contourf(x, y, z, alpha = 1)
        plt.colorbar(cs)
        show_2D(ax)        
    elif function_type == 'I':
        f_list = errorbar()
        x = np.linspace(f_list[1], f_list[2], 5)  
        exec(f_list[0]) #Run the code from the input function
        ax = plt.axes()
        ax.errorbar(x, y, xerr = f_list[3], yerr = f_list[4])
        show_2D(ax)
    elif function_type == 'J':
        f_list = vague_contours()
        xrange = np.linspace(f_list[1], f_list[2], 500)  
        yrange = np.linspace(f_list[3], f_list[4], 500)   
        x, y = np.meshgrid(xrange, yrange)
        exec(f_list[0]) #Run the code from the input function
        ax = plt.axes()
        cs = ax.imshow(z, interpolation='bilinear', extent=[f_list[1], f_list[2], f_list[3], f_list[4]], cmap='viridis')
        plt.colorbar(cs)
        show_2D(ax)         
    elif function_type == 'K':
        f_list = _animate_2D_function_()
        x = np.linspace(f_list[1], f_list[2], 500)
        y = np.linspace(f_list[3], f_list[4], 500)
        fig, ax = plt.subplots()
        line_plot, = ax.plot(x, y)  
        code_string = "line_plot.set_ydata(" + f_list[0] + ")"
        def Func(t):     
            exec(code_string) #Run the code from the input function
            return line_plot,
        ani = animation.FuncAnimation(fig, Func, interval = 10, blit = True)
        ani.save("2D_function_animation.gif")
        show_2D(ax)
    elif function_type == 'L':
        f_list = _3D_surface_function_()
        xrange = np.linspace(f_list[1], f_list[2], 500)  
        yrange = np.linspace(f_list[3], f_list[4], 500)   
        x, y = np.meshgrid(xrange, yrange)
        exec(f_list[0]) #Run the code from the input function
        ax = plt.axes(projection = '3d')
        cs = ax.plot_surface(x, y, z, cmap = 'viridis')
        plt.colorbar(cs)
        show_3D(ax)             
    elif function_type == 'M':
        f_list = _3D_parametric_function_()
        t = np.linspace(f_list[2], f_list[3], 500)
        exec(f_list[0]) #Run the code from the input function
        exec(f_list[1]) #Run the code from the input function
        ax = plt.axes(projection = '3d')
        ax.plot3D(x, y, t)
        show_3D(ax)          
    elif function_type == 'N':
        f_list = _3D_t_vector_field_()
        t = np.linspace(f_list[3], f_list[4], 5)
        xrange, yrange, zrange = np.meshgrid(t, t, t)
        exec(f_list[0]) #Run the code from the input function
        exec(f_list[1]) #Run the code from the input function
        exec(f_list[2]) #Run the code from the input function
        ax = plt.axes(projection = '3d')
        ax.quiver(xrange, yrange, zrange, x, y, z, length = (f_list[4] - f_list[3]) / 10, normalize=True)
        show_3D(ax)    
    elif function_type == 'O':
        f_list = _3D_xyz_vector_field_()
        xrange = np.linspace(f_list[3], f_list[4], 5)
        yrange = np.linspace(f_list[5], f_list[6], 5)
        zrange = np.linspace(f_list[7], f_list[8], 5)
        x, y, z= np.meshgrid(xrange, yrange, zrange)
        exec(f_list[0]) #Run the code from the input function
        exec(f_list[1]) #Run the code from the input function
        exec(f_list[2]) #Run the code from the input function
        ax = plt.axes(projection = '3d')
        ax.quiver(x, y, z, v1, v2, v3, length = (f_list[4] - f_list[3]) / 10, normalize=True)
        show_3D(ax)          
    elif function_type == 'P':
        f_list = _3D_surface_animation_()
        xrange = np.linspace(f_list[1], f_list[2], 50)
        yrange = np.linspace(f_list[3], f_list[4], 50)
        x, y = np.meshgrid(xrange, yrange)
        z = np.zeros((50, 50, f_list[5]))        
        code_string = "f = lambda x, y, t: " + f_list[0]
        exec(code_string) #Run the code from the input function
        for i in range(f_list[5]):
            z[:, :, i] = f(x, y, i)        
        def change_frame(frame, z, plot): 
            plot[0].remove()
            plot[0] = ax.plot_surface(x, y, z[:, :, frame], cmap="viridis")
        fig = plt.figure()
        ax = plt.axes(projection = '3d')        
        plot = [ax.plot_surface(x, y, z[:, :, 0], rstride=1, cstride=1)]
        ani = animation.FuncAnimation(fig, change_frame, f_list[5], fargs=(z, plot), interval=100)
        ani.save('3D_surface_animation.gif')
        show_3D(ax)    
    elif function_type == 'Z':
        function_type = 'Z'
        print("The program ends")