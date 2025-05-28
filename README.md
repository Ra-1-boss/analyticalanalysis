# analyticalanalysis
syms Id Vd R Is Vt Vsrc
% Diode Shockley equation: Id = Is*(exp(Vd/Vt) - 1)
diode_eqn = Id == Is * (exp(Vd/Vt) - 1);  

% KVL: Vsrc = Vd + Id*R
kvl_eqn = Vsrc == Vd + Id * R;  

% Solve system of equations
sol = solve([diode_eqn, kvl_eqn], [Id, Vd]);  

% Display simplified results
disp('Diode Current (Id):');
pretty(simplify(sol.Id));
disp('Diode Voltage (Vd):');
pretty(simplify(sol.Vd));  

% Example with realistic values
Is_real = 1e-12;  % Reverse saturation current (A)
Vt_real = 0.026;  % Thermal voltage (V)
Vsrc_real = 5;     % Supply voltage (V)
R_real = 1e3;     % Resistance (Ohms)  

% Substitute values and solve numerically
Id_num = double(subs(sol.Id, {Is, Vt, Vsrc, R}, ...
                    {Is_real, Vt_real, Vsrc_real, R_real}));
fprintf('\nNumerical Solution: Id = %.4f mA\n', Id_num*1e3);
