\def\P{\mathbb{P}}
\def\N{\mathbb{N}}
\def\R{\mathbb{R}}
\def\E{\mathbb{E}}
\def\Z{\mathbb{Z}}

%----------------------------------------------------------------------------------------

\documentclass{beamer}

\usepackage{amsmath}
\usepackage{amsfonts}
\usepackage{graphicx}
\usepackage{bbm}
\usepackage{amssymb}             %数学公式
\usepackage{amsfonts}            %数学字体
\usepackage{mathrsfs}
\usepackage{float} 
\usepackage{shuffle}

\usepackage{amsthm}
\usepackage{t1enc}
\usepackage{subfig}


\let\algorithm\relax
\let\endalgorithm\relax
\usepackage[linesnumbered,ruled,vlined]{algorithm2e}%[ruled,vlined]{
\usepackage{algpseudocode}
\renewcommand{\algorithmicrequire}{\textbf{Input:}}  % Use Input in the format of Algorithm
\renewcommand{\algorithmicensure}{\textbf{Output:}} % Use Output in the format of Algorithm 

\mode<presentation> {

% The Beamer class comes with a number of default slide themes
% which change the colors and layouts of slides. Below this is a list
% of all the themes, uncomment each in turn to see what they look like.

%\usetheme{default}
%\usetheme{AnnArbor}
%\usetheme{Antibes}
%\usetheme{Bergen}
%\usetheme{Berkeley}
%\usetheme{Berlin}
%\usetheme{Boadilla}
%\usetheme{CambridgeUS}
%\usetheme{Copenhagen}
%\usetheme{Darmstadt}
%\usetheme{Dresden}
%\usetheme{Frankfurt}
%\usetheme{Goettingen}
%\usetheme{Hannover}
%\usetheme{Ilmenau}
%\usetheme{JuanLesPins}
%\usetheme{Luebeck}
\usetheme{Madrid}
%\usetheme{Malmoe}
%\usetheme{Marburg}
%\usetheme{Montpellier}
%\usetheme{PaloAlto}
%\usetheme{Pittsburgh}
%\usetheme{Rochester}
%\usetheme{Singapore}
%\usetheme{Szeged}
%\usetheme{Warsaw}

% As well as themes, the Beamer class has a number of color themes
% for any slide theme. Uncomment each of these in turn to see how it
% changes the colors of your current slide theme.

%\usecolortheme{albatross}
%\usecolortheme{beaver}
%\usecolortheme{beetle}
%\usecolortheme{crane}
%\usecolortheme{dolphin}
%\usecolortheme{dove}
%\usecolortheme{fly}
%\usecolortheme{lily}
%\usecolortheme{orchid}
%\usecolortheme{rose}
%\usecolortheme{seagull}
%\usecolortheme{seahorse}
%\usecolortheme{whale}
%\usecolortheme{wolverine}

%\setbeamertemplate{footline} % To remove the footer line in all slides uncomment this line
\setbeamertemplate{footline}[page number] % To replace the footer line in all slides with a simple slide count uncomment this line

\setbeamertemplate{navigation symbols}{} % To remove the navigation symbols from the bottom of all slides uncomment this line
}

\usepackage{graphicx} % Allows including images
\usepackage{booktabs} % Allows the use of \toprule, \midrule and \bottomrule in tables

%----------------------------------------------------------------------------------------
%	TITLE PAGE
%----------------------------------------------------------------------------------------

\title[Stage de recherche]{\small DATA DYNAMICS LEARNING AND MODEL-FREE PRICING} % The short title appears at the bottom of every slide, the full title is only on the title page

\author{Intern: Xinkai TANG \\ Tutor: Nadhem MEZIOU} % Your name
\institute[Natixis] % Your institution as it will appear on the bottom of every slide, may be shorthand to save space
{
Natixis \\ % Your institution for the title page
\medskip
\textit{} % Your email address
}
\date{September 2022} % Date, can be changed to a custom date

\begin{document}

\begin{frame}
\titlepage % Print the title page as the first slide
\end{frame}

\begin{frame}
\frametitle{Overview} % Table of contents slide, comment this block out to remove it
\tableofcontents % Throughout your presentation, if you choose to use \section{} and \subsection{} commands, these will automatically be printed on this slide as an overview of your presentation
\end{frame}

%----------------------------------------------------------------------------------------
%	PRESENTATION SLIDES
%----------------------------------------------------------------------------------------

%------------------------------------------------
%\section{Question 1} % Sections can be created in order to organize your presentation into discrete blocks, all sections and subsections are automatically printed in the table of contents as an overview of the talk
%------------------------------------------------

%\subsection{Subsection Example} % A subsection can be created just before a set of slides with a common theme to further break down your presentation into chunks
\section{Introduction}
\subsection{Tensor algebra}

\begin{frame}
\frametitle{Introduction with examples}
\textbf{Path integrals}

For a path $X:[a,b]\rightarrow \R^d$:
$$
S(X)^i_{a,t} = \int_{a<s<t}dX_t^i=X_t^i-X_a^i,
$$
$$
S(X)_{a,t}^{i,j}=\int_{a<s<t}S(X)_{a,s}^idX_s^j=\int_{a<r<s<t}dX_r^idX_s^j.
$$
For any integer $k\geq 1$ and collection of indexes $i_1,\cdots,i_k\in \{1,\cdots,d\}$, we define
$$
S(X)_{a,t}^{i_1,\cdots,i_k} = \int_{a<t_k<t}\cdots\int_{a<t_1<t_2}dX_{t_1}^{i_1}\cdots dX_{t_k}^{i_k}.
$$
\begin{definition}[Signatures]
The $signature$ of a path $X: [a,b]\rightarrow\R^d$ is the collection (infinite series) of all the iterated integrals of $X$.
$$
S(X)_{a,b} = (1,S(X)^1_{a,b},\cdots,S(X)_{a,b}^d,S(X)_{a,b}^{1,1},S(X)_{a,b}^{1,2},\cdots)
$$
\end{definition}
\end{frame}

\begin{frame}
\frametitle{European option example}
Approximation of a European call of maturity $T$ by a linear combination of signature payoffs.
\begin{figure}[htbp]
\centering
\subfigure{
\begin{minipage}[t]{0.45\linewidth}
\centering
\includegraphics[width=\textwidth]{./images/test1.png}
%\caption{fig1}
\end{minipage}%
}%
\subfigure{
\begin{minipage}[t]{0.45\linewidth}
\centering
\includegraphics[width=\textwidth]{./images/test2.png}
%\caption{fig2}
\end{minipage}%
}%
\centering
\caption{The left graph shows the approximation of Call Payoff by 5-th order polynomials.The left graph shows the approximation of Call Payoff by 10-th order polynomials.}
%\label{fig2}
\end{figure}
\end{frame}

\begin{frame}
\frametitle{Introduction to the tensor algebra}
Consider the $d$-diemnsional Euclidean space $\R^d$. We denote by $(\R^d)^{\otimes n}:= \underbrace{\R^d\otimes\cdots\otimes\R^d}_n$, where $\otimes$ is the tensor product.
\begin{block}{Definition: Extended tensor algebra}
$$
T((\R^d)):= \{(a_0,a_1,\cdots,a_n,\cdots)|a_n\in(\R^d)^{\otimes n}, \forall n \geq 0\}.
$$
\end{block}

For $\textbf{a} = (a_i)_{i=0}^\infty,\textbf{b} = (b_i)_{i=0}^\infty\in T((\R^d))$ by:
\begin{equation*}
    \begin{split}
    \textbf{a}+\textbf{b}&:=(a_i+b_i)_{i=0}^\infty,\\
    \textbf{a}\otimes\textbf{b}&:=\left(\sum_{k=0}^i a_k\otimes b_{i-k}\right)_{i=0}^\infty.\\
    \end{split}
\end{equation*}
\end{frame}

\begin{frame}
\frametitle{Introduction to the tensor algebra}
\begin{block}{Definition: Shuffle product}
Let $I=(i_1,\cdots,i_n)\in\{1,\cdots,d\}^n,J=(j_1,\cdots,j_m)\in\{1,\cdots,d\}^m$. The shuffle product of $e_I^*$ and $e_J^*$, denoted by $\shuffle$, is defined by:
$$
e_I^*\shuffle e_J^*:=(e_{(i_1,\cdots,i_{n-1})}^*\shuffle e_J^*)\otimes e_{i_n}^*+(e_I^*\shuffle e_{(j_1,\cdots,j_{m-1})}^*)\otimes e_{j_m}^*,
$$
and $e_I^*\shuffle \mathbf{1}^* = \mathbf{1}^*\shuffle e_I^* = e_I^*$.
\end{block}
Consider the alphabet $\mathcal{A}_d:=\{1,2,\cdots,d\}$, which consists of $d$ letters. We have the following identification:
$$
e_{i_1}^*\otimes\cdots e_{i_n}^*\in T((\R^d)^*)\leftrightarrow\textcolor{blue}{\mathbf{i_1}}\cdots\textcolor{blue}{\mathbf{i_n}}\in\mathcal{W}(\mathcal{A}_d),
$$
where $\mathcal{W}(\mathcal{A}_d)$ is the real vector space of all words with alphabet $\mathcal{A}_d$.
\begin{block}{Example}
Consider the alphabet $\mathcal{A}_3=\{\textcolor{blue}{\mathbf{1,2,3}}\}$. If $\textcolor{blue}{\mathbf{w}}=\textcolor{blue}{\mathbf{123}},\textcolor{blue}{\mathbf{v}}=\textcolor{blue}{\mathbf{32}},\textcolor{blue}{\mathbf{u}}=\textcolor{blue}{\mathbf{3}}$, then $(\textcolor{blue}{\mathbf{w}}+\textcolor{blue}{\mathbf{v}})\textcolor{blue}{\mathbf{u}}=\textcolor{blue}{\mathbf{1233}}+\textcolor{blue}{\mathbf{323}}$.
\end{block}
\end{frame}

\begin{frame}
\frametitle{Introduction to the tensor algebra}
\begin{block}{Definition: Shuffle product}
The shuffle product $\shuffle$ : $\mathcal{W}(\mathcal{A}_d)\times\mathcal{W}(\mathcal{A}_d)\rightarrow\mathcal{W}(\mathcal{A}_d)$ is defined by:
$$
\textcolor{blue}{\mathbf{ua}}\shuffle\textcolor{blue}{\mathbf{vb}}=(\textcolor{blue}{\mathbf{u}}\shuffle\textcolor{blue}{\mathbf{vb}})\textcolor{blue}{\mathbf{a}}+(\textcolor{blue}{\mathbf{ua}}\shuffle\textcolor{blue}{\mathbf{v}})\textcolor{blue}{\mathbf{b}},
$$
$$
\textcolor{blue}{\mathbf{w}}\shuffle\textcolor{blue}{\varnothing}=\textcolor{blue}{\varnothing}\shuffle\textcolor{blue}{\mathbf{w}}=\textcolor{blue}{\mathbf{w}},
$$
where $\textcolor{blue}{\mathbf{u}},\textcolor{blue}{\mathbf{v}}$ are words and $\textcolor{blue}{\mathbf{a}},\textcolor{blue}{\mathbf{b}}$ are letters.
\end{block}
\begin{block}{Example}
Consider the alphabet $\mathcal{A}_3=\{\textcolor{blue}{\mathbf{1,2,3}}\}$ for example, we have $\textcolor{blue}{\mathbf{12}}\shuffle\textcolor{blue}{\mathbf{34}} = \textcolor{blue}{\mathbf{1234}}+\textcolor{blue}{\mathbf{1324}}+\textcolor{blue}{\mathbf{1342}}+\textcolor{blue}{\mathbf{3124}}+\textcolor{blue}{\mathbf{3142}}+\textcolor{blue}{\mathbf{3412}}$.
\end{block}
\end{frame}

\subsection{Signatures}
\begin{frame}
\frametitle{Introduction to the signatures}
\begin{block}{Definition: Signatures of a path}
Given a path $Z\in \mathcal{C}([0,T];\R^d)$ with bounded variation,
$$
\Z^{<\infty}_{s,t} := (1,\Z^1_{s,t},\cdots,\Z^n_{s,t},\cdots)
$$
where
$$
\Z^n_{s,t}:=\int_{s<u_1<\cdots<u_k<t}dZ_{u_1}\otimes\cdots\otimes dZ_{u_k}\in(\R^d)^{\otimes n}.
$$
The truncated signature $\Z^{\leq N}_{s,t}:=(1,\Z^1_{s,t},\cdots,\Z^N_{s,t}).$
\end{block}
\begin{block}{Example}
Consider a path $Z=(Z^1,Z^2)$ on $\R^2$.
\begin{enumerate}
\item $\left\langle \textcolor{blue}{\mathbf{22}},\Z^{<\infty}_{0,T} \right\rangle = \int_0^T\int_0^t\circ dZ_s^2\circ dZ_t^2 = \frac{1}{2}(Z_T^2-Z_0^2)^2$.
\item For $l\in T((\R^2)^*)$, $\left\langle l\textcolor{blue}{\mathbf{1}},\Z^{<\infty}_{0,T} \right\rangle = \int_0^T\left\langle l,\Z^{<\infty}_{0,t} \right\rangle \circ dZ_t^1.$
\end{enumerate}
\end{block}
\end{frame}

\begin{frame}
\frametitle{Introduction to the signatures}
\begin{block}{Definition: Logarithm map}
Let $a = (a_0,a_1,\cdots)\in T((\R^d))$ such that $a_0=1$ and $t=a-1$. Then the logarithm map denoted by $\mathrm{log}$ is defined as follows:
$$
\mathrm{log}(a) = \mathrm{log}(1+t)=\sum_{n=1}^{\infty}\frac{(-1)^{n-1}}{n}t^{\otimes n},\forall a\in T((\R^d)).
$$
\end{block}
\begin{block}{Definition: Log-signatures}
Let $Z:[0,T]\rightarrow \R^d$ be a path with a well-defined signature $\Z^{<\infty}_{0,T}$. The log-signature is then defined by
$$
\mathrm{log}\Z^{<\infty}_T := -\Z^{<\infty}_T +\frac{1}{2}(\Z^{<\infty}_T)^{\otimes 2}-\frac{1}{3}(\Z^{<\infty}_T)^{\otimes 3}+\cdots+(-1)^n\frac{1}{n}(\Z^{<\infty}_T)^{\otimes n}+\cdots.
$$
\end{block}
\end{frame}

\begin{frame}
\frametitle{Introduction to the signatures}
\begin{block}{Theorem: Uniqueness}
Under certain assumptions, a path is uniquely determined by its signature.
\end{block}

\vspace{5mm}
\begin{block}{Theorem: Expected signature characterises the law of a process}
Let $Z:[0,T]\rightarrow \R^d$ be a stochastic process such that its signature $\Z^{<\infty}_{0,T}$ is a.s. well-defined. Assume that its expected signature $\E[\Z^{<\infty}_{0,T}]$ is well-defined. Under certain assumptions $\E[\Z^{<\infty}_{0,T}]$ uniquely determines the law of the stochastic process $Z$.
\end{block}
\end{frame}

\section{Practical applications}
\subsection{Data-driven market simulator for small data environment}
\begin{frame}
\frametitle{Data-driven market simulator for small data environment}
\textbf{Classical modelling}: stochastic market models, autoregressive models, etc. They are tractable but less flexible.

\vspace{5mm}
\textbf{Data-driven modern generative modelling}: explicit knowledge not required. Popular models are Generative Adversarial Networks (GAN) and Variational Autoencoders (VAE).

\vspace{5mm}
\textbf{Why we use log-signatures and VAE?}

\vspace{1mm}
Log-signatures span a linear space. Log-signatures represent the data with a lower dimension.

\vspace{1mm}
VAE are adapted to the small data.
\end{frame}

\begin{frame}
\frametitle{Data-driven market simulator for small data environment}
\textbf{\large Problem formation}:

\vspace{2mm}
We denote the log return as:
$$
r(t,\Delta_t):=X(t+\Delta_t)-X(t),
$$
The purpose is to make $M\in\N$ returns-sequences of length $k$ for a suitable $k\in\N$
$$
(\tilde{r_1}(t_1,\Delta_t))\cdots\tilde{r_1}(t_k,\Delta_t)),\cdots,(\tilde{r_M}(t_1,\Delta_t))\cdots\tilde{r_M}(t_k,\Delta_t)),
$$
as "similar" as possible to the observed $k$-sequences of returns with $N\in\N$ observations
$$
(r_1(t_1,\Delta_t))\cdots r_1(t_k,\Delta_t)),\cdots,(r_N(t_1,\Delta_t))\cdots r_N(t_k,\Delta_t)),
$$
\end{frame}

\begin{frame}
\frametitle{Data-driven market simulator for small data environment}
\textbf{\large Maximum Mean Discrepancy}

\vspace{2mm}
Determine if two samples are drawn from different distributions $p$ and $q$ based on MMD.
$$
\mathrm{MMD}[\mathcal{H},p,q]:=\sup_{f\in\mathcal{H}}(\E_{X\sim p}[f(X)]-\E_{Y\sim q}[f(Y)]).
$$
For a sample of real paths $Y_1,\cdots,Y_n$ and a sample from the generative model $X_1,\cdots,X_n$, we computed the signature-based MMD test statistic $T(X_1,\cdots,X_n;Y_1,\cdots,Y_n)$, we accept if $T_U^2<c_\alpha = 4\sqrt{-\mathrm{log}\alpha/n}$
\begin{align*}
T(X_1,\cdots,X_n;Y_1,\cdots,Y_n) &:= \frac{1}{n(n-1)}\sum_{i,j;i\neq j}k(X_i,X_j) \\
&-\frac{2}{n^2}\sum_{i,j}k(X_i,Y_j) \\
&+\frac{1}{n(n-1)}\sum_{i,j;i\neq j}k(Y_i,Y_j),
\end{align*}
\end{frame}

\begin{frame}
\frametitle{Data-driven market simulator for small data environment}
\textbf{\large Main steps}:

\vspace{2mm}
\begin{enumerate}
\item Data extraction from time series.
\item Preprocessing the data.
\item Creating and training the VAE.
\item Postprocessing of the outputs of the VAEs.
\item Performance evaluation.
\end{enumerate}
\end{frame}

\begin{frame}
\frametitle{Data-driven market simulator for small data environment}
\begin{block}{Definition: Lead-lag transformation}
Given a path $Z$ with dimension 1. Define its lead-lag transformation $\hat{Z}:[0,1]\rightarrow \R^2$ of the continuous path given by
$$
\hat{Z}_{2k/2n}:=(Z_{t_k},Z_{t_k})\in\R^2, \quad \hat{Z}_{(2k+1)/2n}:=(Z_{t_k},Z_{t_{k+1}})\in\R^2
$$
and linear interpolation in between. 
\end{block}
\begin{block}{Example}
Consider a path $Z=(1,4,6,2)$, the Lead-Lag mapping is given by:
\begin{equation*}
Z^{LL}=\left\{
\begin{aligned}
Z^{Lag} = \{1,1,4,4,2,2,6\}, \\
Z^{Lead} = \{1,4,4,2,2,6,6\}.
\end{aligned}
\right.
\end{equation*}

\end{block}

%\begin{figure}[htbp]
%\centering
%\begin{minipage}[t]{\linewidth}
%\centering
%\includegraphics[height=0.5\textheight]{./images/leadlag.png}
%%\caption{fig1}
%\end{minipage}%
%\centering
%\label{fig8}
%\end{figure}
\end{frame}


\begin{frame}
\frametitle{Data-driven market simulator for small data environment}
\begin{figure}[htbp]
\centering

\subfigure{
\begin{minipage}[t]{0.46\linewidth}
\centering
\includegraphics[height=0.4\textheight]{./images/proj1.png}
%\caption{fig1}
\end{minipage}%
}%
\subfigure{
\begin{minipage}[t]{0.45\linewidth}
\centering
\includegraphics[height=0.4\textheight]{./images/proj2.png}
%\caption{fig2}
\end{minipage}%
}%
                 %这个回车很重要 \quad也可以
\subfigure{
\begin{minipage}[t]{0.46\linewidth}
\centering
\includegraphics[height=0.4\textheight]{./images/proj3.png}
%\caption{fig1}
\end{minipage}%
}%
\subfigure{
\begin{minipage}[t]{0.45\linewidth}
\centering
\includegraphics[height=0.4\textheight]{./images/proj4.png}
%\caption{fig2}
\end{minipage}%
}%

\centering
\caption{The image shows projections of generated signatures.}
\label{fig1}
\end{figure}
\end{frame}

\begin{frame}
\frametitle{Data-driven market simulator for small data environment}
\begin{figure}[htbp]
\centering
\subfigure{
\begin{minipage}[t]{0.44\linewidth}
\centering
\includegraphics[width=\textwidth]{./images/datadriven_invert1.png}
%\caption{fig1}
\end{minipage}%
}%
\subfigure{
\begin{minipage}[t]{0.46\linewidth}
\centering
\includegraphics[width=\textwidth]{./images/datadriven_invert2.png}
%\caption{fig2}
\end{minipage}%
}%
\centering
\caption{The left graph shows the generated paths inverted from log-signatures. The right graph shows the histogram of monthly returns.}
\label{fig2}
\end{figure}
 We applied the MMD test on paths and the KS-test on marginal distribution at the final time. Results show that the model can be accepted with a confidence level $\alpha>0.999$ and that the KS-test has a $p$-value of 0.626.
\end{frame}


\subsection{Model-free pricing of exotic derivatives}

\begin{frame}
\frametitle{Model-free pricing of exotic derivatives}
Augmentation of path $X$ : $\hat{X}_t:=(t,X_t)\in\R^2$ and its lead-lag transformation $\hat{X}^{LL}:=(t_{lag},X_{lag},t_{lead},X_{lead})$.

\begin{block}{Definition: Linear signature payoff}
A payoff $G:\hat{\Omega}^{LL}\rightarrow\R$ is a linear signature payoff if there is a linear functional $g\in T((\R^4)^*)$ such that
$$
G(\hat{X}^{LL,<\infty}) = \left\langle g,\hat{\mathbb{X}}^{LL,<\infty} \right\rangle.
$$
\end{block}
\begin{block}{Example}
\begin{enumerate}
\item Let $K\in\R$, and set $g=(1-K)\textcolor{blue}{\varnothing}+\textcolor{blue}{\mathbf{2}}$. Then, we have $\left\langle g,\hat{\mathbb{X}}^{LL,<\infty} \right\rangle = 1-K+X_T-X_0=X_T-K$.
\item Let $K\in\R$. Set $g=(1-K)\textcolor{blue}{\varnothing} + \frac{1}{T}\textcolor{blue}{\mathbf{21}}$. Then, $\left\langle g,\hat{\mathbb{X}}^{LL,<\infty} \right\rangle = 1-K+\frac{1}{T}\int_0^T(X_s-X_0)ds=\frac{1}{T}\int_0^TX_sds-K$.
\end{enumerate}
\end{block}

\end{frame}

\begin{frame}
\frametitle{Model-free pricing of exotic derivatives}
\begin{block}{Proposition}
Let $G:\hat{\Omega}^{LL}\rightarrow\R$ be a continuous payoff. Given any $\epsilon>0$, there exists a compact set $\mathcal{K}_\epsilon\subset\hat{\Omega}_T$ and $g\in T((\R^4)^*)$ such that:
\begin{enumerate}
\item $\mathbb{P}[\mathcal{K}_\epsilon]>1-\epsilon$,
\item $|G(\hat{\mathbb{X}}^{LL,<\infty})-\left\langle g,\hat{\mathbb{X}}^{LL,<\infty} \right\rangle|<\epsilon\quad\forall\hat{\mathbb{X}}^{LL,<\infty}\in\mathcal{K}_\epsilon$.
\end{enumerate}
\end{block}
We approximate the price of the payoff $G$ by a linear signature payoff:
$$
Z_T\E^{\mathbb{Q}}[G(\hat{\mathbb{X}}^{LL,<\infty})]\approx Z_T\left\langle l,\E^{\mathbb{Q}}[\hat{\mathbb{X}}^{LL,<\infty}] \right\rangle\quad \textrm{with}\quad l\in T((\R^4)^*).
$$
Assume that one has access to a family of pairs $\{(G_i,p_i)\}_i$ of payoffs $G_i$ with market prices $p_i$.
$$
Z_T\left\langle l_i,\E^{\mathbb{Q}}[\hat{\mathbb{X}}^{LL,<\infty}] \right\rangle \approx p_i\quad\textrm{for each}\quad i.
$$
One can apply linear regression to estimate the (discounted) implied expected signatures $Z_T\E^{\mathbb{Q}}[\hat{\mathbb{X}}^{LL,<N}]$.
\end{frame}

\begin{frame}
\frametitle{Model-free pricing of exotic derivatives}
\textbf{Simulation 1: Heston Model}

The payoff types considered are European call options, barrier up-in calls, Asian calls, and forward contracts.
\begin{figure}[htbp]
\centering
\begin{minipage}[t]{0.6\linewidth}
\centering
\includegraphics[width=\textwidth]{./images/nonpara_v2_titre.png}
%\caption{fig1}
\end{minipage}%
\centering
\caption{Predicted market prices using the implied expected signatures, and the real market prices. Blue points are predicted market prices and orange points are real market prices. $R^2 = 0.9945$. The estimated rate $\hat{r}=2.08\%$.}
\label{fig1}
\end{figure}
\end{frame}

\begin{frame}
\frametitle{Model-free pricing of exotic derivatives}
\textbf{Simulation 1: Heston Model}

Due to the lead-lag transformation, there are several terms of signatures that are the same, which may cause a problem when we do linear regression.
\begin{figure}[htbp]
\centering
\begin{minipage}[t]{0.6\linewidth}
\centering
\includegraphics[width=\textwidth]{./images/nonpara_v5.png}
%\caption{fig1}
\end{minipage}%
\centering
\caption{Predicted market prices using the implied expected signature, and the real market prices. $R^2=0.9960.$}
\label{fig2}
\end{figure}
\end{frame}

\begin{frame}
\frametitle{Model-free pricing of exotic derivatives}
\textbf{Simulation 2: GARCH, Hull-White, Rough Volatility Model}
\begin{figure}[htbp]
\centering

\subfigure{
\begin{minipage}[t]{0.4\linewidth}
\centering
% \includegraphics[width=\textwidth]{./images/num_v41_1.png}
\includegraphics[height=0.25\textheight]{./images/num_v41_1.png}
%\caption{fig1}
\end{minipage}%
}%
\subfigure{
\begin{minipage}[t]{0.4\linewidth}
\centering
% \includegraphics[width=\textwidth]{./images/num_v41_2.png}
\includegraphics[height=0.25\textheight]{./images/num_v41_2.png}
%\caption{fig2}
\end{minipage}%
}%
                 %这个回车很重要 \quad也可以
\subfigure{
\begin{minipage}[t]{0.4\linewidth}
\centering
% \includegraphics[width=\textwidth]{./images/num_v5_1.png}
\includegraphics[height=0.25\textheight]{./images/num_v5_1.png}
%\caption{fig1}
\end{minipage}%
}%
\subfigure{
\begin{minipage}[t]{0.4\linewidth}
\centering
% \includegraphics[width=\textwidth]{./images/num_v5_2.png}
\includegraphics[height=0.25\textheight]{./images/num_v5_2.png}
%\caption{fig2}
\end{minipage}%
}%

\subfigure{
\begin{minipage}[t]{0.4\linewidth}
\centering
% \includegraphics[width=\textwidth]{./images/num_v6_1.png}
\includegraphics[height=0.25\textheight]{./images/num_v6_1.png}
%\caption{fig1}
\end{minipage}%
}%
\subfigure{
\begin{minipage}[t]{0.4\linewidth}
\centering
% \includegraphics[width=\textwidth]{./images/num_v6_2.png}
\includegraphics[height=0.25\textheight]{./images/num_v6_2.png}
%\caption{fig2}
\end{minipage}%
}%

\centering
\caption{$R^2$ scores of all the models tested reached 99\%.}
\label{fig5}
\end{figure}

\end{frame}

\begin{frame}
\frametitle{Model-free pricing of exotic derivatives}
\textbf{Simulation 2: GARCH, Hull-White, Rough Volatility Model}
\begin{figure}[htbp]
\centering

\subfigure{
\begin{minipage}[t]{0.46\linewidth}
\centering
\includegraphics[height=0.32\textheight]{./images/num_v42_1.png}
%\caption{fig1}
\end{minipage}%
}%
\subfigure{
\begin{minipage}[t]{0.45\linewidth}
\centering
\includegraphics[height=0.32\textheight]{./images/num_v42_2.png}
%\caption{fig2}
\end{minipage}%
}%
                 %这个回车很重要 \quad也可以
\subfigure{
\begin{minipage}[t]{0.46\linewidth}
\centering
\includegraphics[height=0.32\textheight]{./images/num_v42_3.png}
%\caption{fig1}
\end{minipage}%
}%
\subfigure{
\begin{minipage}[t]{0.45\linewidth}
\centering
\includegraphics[height=0.32\textheight]{./images/num_v42_4.png}
%\caption{fig2}
\end{minipage}%
}%

\centering
\caption{The first row is the PCA model with 20 principal components and the second row is the PCA model with 12 principal components (which explained 99.9\% de variance).}
\label{fig6}
\end{figure}

\end{frame}

\section{Conclusion}
\begin{frame}
\frametitle{Conclusion}
Based on the signatures method, we can:
\begin{enumerate}
\item Generate new data provided with small training data.
\item Estimate the implied expected signatures using prices of market.
\item Price other derivatives.
\end{enumerate}
\end{frame}


\begin{frame}
\Huge{\centerline{Merci!}}
\end{frame}

\end{document}